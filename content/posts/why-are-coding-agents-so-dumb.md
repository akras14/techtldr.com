---
title: "Coding agents lag the models they wrap: tasks, planning, sandboxing"
slug: "why-are-coding-agents-so-dumb"
date: 2026-10-09T22:06:33+0000
summary: "Michael Lynch argues that coding agents (the harnesses around the models, like Claude Code, Codex and OpenCode) lag far behind the models themselves. He lists basic gaps in task management, delegation, planning, sandboxing and self-knowledge, and guesses that vendors optimize for demos and benchmarks that measure models rather than agents."
source: "https://mtlynch.io/why-are-coding-agents-so-dumb/"
source_title: "Why Are Coding Agents So Dumb?"
source_author: "Michael Lynch"
source_site: "mtlynch.io"
hn_url: "https://news.ycombinator.com/item?id=50020947"
---
Lynch distinguishes the model (the LLM) from the agent (the software that wires it to files and commands). Since early 2025 the models have improved a lot, he says, but the agents have not, and they are now the bottleneck.

His main complaints:

- **Poor task management.** An agent split a 1.5k-line feature into 10 subtasks and then ran them one at a time; Claude Code uses only a couple of subagents and idles while tests run.
- **No delegation.** Agents don't hand cheap grunt work to a cheaper model or escalate hard problems to a smarter one, so the user ends up managing models by hand.
- **Ignorance of themselves.** Agents search the web to learn their own features.
- **Unreadable plans.** Plans come out as a jumble of low-level details instead of a top-down or bottom-up outline.
- **Stalling.** One agent sat idle overnight after asking what to name a git branch.
- **No real sandbox.** Permission settings are polite requests the model can ignore, not OS-level boundaries, even though sandboxing tools have existed for over a decade. Lynch built his own.

His wish list: per-task model routing tuned for cost, speed and correctness; human-friendly plans with diagrams; deterministic OS-level sandboxing scoped to one repo; an agent that knows its own docs; any LLM provider; open source; an "AFK mode" that makes decisions after a timeout; and extras such as a local web dashboard, ETAs, a language-aware diff view, secret-injecting proxies and cross-model code review.

His hypothesis for the neglect is a principal-agent problem. Executives choose direction based on demos and benchmarks, which mostly measure models, while developers' daily pain around security and efficiency goes unmeasured. He admits this explanation isn't fully satisfying.
