---
title: "Agent memory plugins are just RAG; give agents maintained docs instead"
slug: "agents-need-documentation"
date: 2026-10-10T21:31:50+0000
summary: "Memory plugins for coding agents mostly boil down to RAG over transcript snippets, which surfaces whatever is similar rather than what is correct or current. The author argues agents need a maintained documentation workspace instead, read before each task and updated after it, and has open-sourced their version, Operator."
source: "https://liao.gg/blog/agents-dont-need-memory"
source_title: "Agents Don’t Need Memory. They Need Documentation."
source_site: "liao.gg"
source_date: "2026-10-03"
hn_url: "https://news.ycombinator.com/item?id=49945933"
---

Memory plugins for coding agents mostly boil down to RAG over transcript snippets, which surfaces whatever is similar rather than what is correct or current. The author argues agents need a maintained documentation workspace instead, read before each task and updated after it, and has open-sourced their version, Operator.

## What "memory" usually is

Most plugins follow the same steps: mine session transcripts, generate snippets, store them in a vector database, inject the top five into each prompt, and maybe give the agent a search tool. Extras like memory tiers, overnight "dreamer" rewrites and rerankers sit on the same base.

## Why recall fails

- **Similarity isn't correctness.** Retrieval can't tell which snippet is right, current, or missing.
- **Snippets lose context**: the motivations and constraints behind a decision.
- **The past is treated as truth**, though the code changes daily.
- **Agents can't search for what they don't know** they're missing.
- **The store is unauditable.** Nobody can see which of 10,000 embeddings are stale or wrong.

People don't rewatch old meetings to recall constraints; they write things down.

## The alternative: a "brain" of docs

A single AGENTS.md is no longer enough for a real project. The author started with an `internal/` folder of specs, plans and indexes in August 2025, which grew into Operator (github.com/aerovato/operator-memory). Its design rules:

- **Contents:** record what code can't tell you: requirements, decisions, constraints, research, standards. Otherwise each session spends about 80k tokens reverse-engineering intent. Example: the Claude Code adapter is under 100 lines because a documented decision pushes logic into the CLI.
- **Discovery:** a lean catalog is injected at session start; the agent opens deeper documents when needed. Generated codebase indexes replace repeated `ls` and `grep`.
- **Freshness:** the working agent writes docs while it has full context, not a background agent, and rewrites facts when they change instead of appending.
- **Discipline:** guidance to record only what code can't provide and keep documents lean. Agents still resist splitting or consolidating, so humans review docs like code.

Because it's Markdown on disk, memory can be diffed, reverted with Git, shared in a committed folder, used from any harness, and needs no vector database or daemons. The author admits they still often have to intervene to create, rewrite or trim documents.
