---
title: "Pi Durable vs. OpenCode: Coding Agents as Durable, Scheduled Tasks"
slug: "pi-durable-vs-opencode"
date: 2026-10-08T06:58:00-07:00
summary: "Most coding harnesses (Claude Code, Codex, OpenCode) are a session plus an in-memory agent loop. Pi Durable instead stores every model call, tool call, and compaction as a durable task that a scheduler runs. Work survives crashes and can move between processes, and harnesses start to look more like operating-system schedulers."
source: "https://x.com/JoshARosen/status/2107840212915077350"
source_title: "Pi Durable vs. OpenCode: An Architectural Comparison"
source_author: "Josh Rosen"
source_site: "X"
source_date: "2026-10-07"
---

Most coding harnesses (Claude Code, Codex, OpenCode) are a session plus an in-memory agent loop. Pi Durable instead stores every model call, tool call, and compaction as a durable task that a scheduler runs. Work survives crashes and can move between processes, and harnesses start to look more like operating-system schedulers.

The author uses OpenCode as the stand-in for the typical session-based design, because it's open source and its internals are easy to inspect.

## 1. Saving execution, not just the conversation

- **OpenCode** persists a lot: the conversation, prompts, tool activity, and session metadata. But the work *currently in progress* lives only in the running process's memory.
- **Pi Durable** turns everything the harness runs into a stored task: each model generation, each tool call, and any task types extensions define. Tasks save checkpoints as they go, so unfinished work stays in storage.
- **Why it matters:** another process can open the same storage, find the unfinished tasks, and carry on.

## 2. A scheduler instead of a loop

In OpenCode, each session's loop drives the work: call the model, run the tools, repeat. In Pi Durable, a **scheduler reads the stored task state** and decides what can run next. Model responses create tool tasks, tasks can wait on other tasks, and finishing one unblocks the next.

Work no longer depends on whichever process kicked it off. It can survive a laptop going to sleep, a container being redeployed, or a machine running out of memory.

## 3. Application state alongside the conversation

OpenCode's stored state mostly exists to run OpenCode itself. Pi Durable adds **typed documents** for data that isn't part of the transcript: plans, tickets, todo lists, sandbox config.

The key detail: **documents are committed atomically with conversation entries and tasks**. An agent can update a plan, record the conversation that changed it, and queue follow-up work in one commit, so a crash can't leave them out of sync. Documents also have defined behavior when a conversation forks: keep the value from the fork point, follow the latest value, or start empty.

## 4. Compaction in the background

Both harnesses keep the full transcript while sending the model a shorter context. In OpenCode, compaction (summarizing older context) is a fixed step in the session lifecycle. In Pi Durable it's **just another task**, so it can:

- start *before* the context limit is reached,
- run in parallel while the agent keeps working, with the summary swapped in at a later turn,
- resume after a crash instead of starting over.

## 5. Delegation as a tree of owned work

OpenCode already has subagents and background child sessions, but the running harness coordinates them. In Pi Durable, **tasks and conversations form an ownership tree**: tasks own child tasks, tools can spawn subagent conversations, and a task can record that it's waiting on others. That enables:

- cancelling a parent and having the cancellation reach everything it owns,
- background work that outlives the foreground work that started it,
- tasks that keep running while the conversation sits idle.

## 6. Tool calls that are safe to recover

Before a tool runs, Pi stores the call as a task, so after a crash it knows whether a call was never started, still running, or finished. That matters once agents have real side effects. Re-reading a file is harmless, but a deploy or a payment may have succeeded even though the harness never got the result. **Tools declare whether they're safe to replay.** If one isn't, Pi gives the interrupted call, with any output already saved, back to the model to decide what to do.

## Where this leads

The author compares it to an operating system: a process exists whether or not it's currently on the CPU, and the scheduler decides what runs. Pi Durable doesn't preempt tasks the way an OS does (yet), but breaking agent work into small durable tasks makes **scheduling a central part of harness design**, not an afterthought.
