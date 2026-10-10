---
title: "Pi Durable runs coding agents as durable scheduled tasks, not sessions"
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

## Beyond the article

*This section is added context from other sources, not part of Rosen's piece.*

**What Pi Durable is.** Earendil shipped it on October 1 as an experimental package next to Pi 1.0 ([announcement](https://earendil.com/posts/pi-durable/)). It's a framework for building any agent app, not a replacement for the Pi coding agent. A few details from the announcement:
- **Small:** about 15,000 lines of TypeScript, so an agent can read the whole thing.
- **Pluggable storage:** in-memory, SQLite, or JSONL. It can run on Bun or inside a Cloudflare Durable Object. One process owns a storage at a time.
- **No double submissions:** a `requestId` makes each submission exactly-once, so a client that retries after a crash gets the original back.
- **Hot-swappable code:** extensions can be replaced while conversations are running.
- **Multiplayer:** several clients can watch and steer the same conversation.

**The question the replies kept asking.** Several readers pointed out a gap in point 6. Handing an interrupted call back to the model just moves the replay decision to the model, and one reply says that in crash tests the model simply called the same tool again. The suggested fix is to use the stored task ID as an idempotency key for the external API. Pi's own announcement already shows this pattern: its payment example passes the task ID to the bank as the charge key, so a rerun after a crash can't charge the card twice. **Takeaway:** declaring a tool non-replayable isn't enough for real side effects. The tool itself has to be idempotent.

Two other replies raised points the article doesn't cover:
- **Permissions:** resuming a task also resumes whatever access it held. Recovery checks what work is left, but not whether the task should still be allowed to do it.
- **Versioning:** stored checkpoints have to stay compatible as the code changes. Pi's task and document definitions carry a version number, but the announcement doesn't describe migrations.

**OpenCode is moving in this direction.** Its [v2 session spec](https://github.com/anomalyco/opencode/blob/dev/specs/v2/session.md) adds:
- a durable inbox for prompts,
- a replayable event log that clients can reconnect to,
- recording tools that were mid-run during a crash as failed, so they're never silently re-run.

But work in progress is still held in process memory, and resuming after a crash is explicitly postponed to a future version. So Rosen's comparison holds today, but the gap is narrowing.
