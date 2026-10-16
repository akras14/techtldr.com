---
title: "Agent harnesses leave 1.6–11.1% of input uncached, and a miss doubles energy"
slug: "harness-cache-breaks-energy"
date: 2026-10-16T23:05:00+0000
summary: "Neuralwatt, an inference provider, analyzed a month of agentic traffic and found the client harness strongly affects how much GPU work a conversation costs. Uncached input ranged from 1.6% to 11.1% across clients, a request with a prompt-cache miss used roughly twice the energy of a cached one, and every cache break in its lab tests came from the harness editing text it had already sent."
source: "https://neuralwatt.com/news/why-harnesses-matter-for-energy-efficiency"
source_title: "Why harnesses matter for energy efficiency"
source_author: "Chad Gibson"
source_site: "Neuralwatt"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50124642"
---

## Bottom line

Neuralwatt, which bills on measured energy, looked at 30 days of mostly agentic traffic (Sept 8 – Oct 7, 2026). Its claim: the client harness decides a surprising share of the work GPUs do, because agents resend a 100k+ token prompt each turn and only an exact unchanged prefix can be served from the prompt cache.

## Findings

- **Cache rates differ by client on the same model and price.** Uncached input ranged from 1.6% to 11.1%. At 92% cached versus 97%, the server does about 2.7 times the prefill work.
- **A miss roughly doubles energy** for a ~50k-token request (about 2.5x on GLM-5.3), and wasted prefill slows other users on the same server.
- **Deep sessions make misses costly.** Past 128k tokens, about 3 turns in 1,000 re-read more than half the prompt; they account for 75% of re-read tokens there, and each costs as much prefill as roughly 600 ordinary turns.
- **Server-side eviction also contributes**, so not every miss is the harness's fault.

## What broke the cache in lab tests

Neuralwatt ran OpenCode, Pi, omp and Kilo Code through a scripted session. Of 539 follow-up requests, 88% were pure appends; the other 65 broke the cache, all because the harness changed earlier content. Idle time, restarts and sub-agents caused none.

- OpenCode 1.x's plan-mode reminder is added and then removed, forcing a re-read of the previous turn. OpenCode 2.x fixes this.
- omp replaces earlier file reads with a "superseded" placeholder. Setting `compaction.supersedeReads: false` and `compaction.dropUseless: false` avoids it.
- Switching reasoning effort changes the prompt at about token 8, so everything is re-read.
- Connecting an MCP server or adding a skill edits the tool list or system prompt; on GLM-5.3 tools render first, so the change lands very early.
- A date in the system prompt forces a re-read once at midnight; compaction often re-reads much of the prompt.

Production metadata showed that changing the tool list or effort level made a cache break more than 20 times as likely. Neuralwatt's takeaway is that most of this is fixable by appending changes instead of editing earlier text.

*Caveat: the author is a vendor; token-level analysis comes from its own test sessions, not customer prompts.*
