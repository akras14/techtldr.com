---
title: "Proton's CTO: LLM use hasn't raised productivity, so engineers now manage AI agents"
slug: "proton-ai-manifesto-engineering"
date: 2026-11-02T11:03:43+0000
summary: "Proton's CTO says despite widespread LLM use, the company hasn't seen real productivity gains, and blames culture, a narrow \"phase 1\" focus on speeding up existing tasks, and a lack of company-wide investment. His fix: engineers act as team leads for AI agents, Proton redesigns the whole development lifecycle around removing bottlenecks, and the company funds the tooling to do it."
source: "https://proton.me/blog/ai-manifesto"
source_title: "Proton's AI manifesto for software engineering"
source_author: "Bart Butler"
source_site: "Proton"
source_date: "2026-11-01"
hn_url: "https://news.ycombinator.com/item?id=50330353"
---

Proton CTO Bart Butler published a public version of an internal engineering memo. His claim is that writing code by hand is over, but engineering and expertise are not. Proton uses LLMs widely yet hasn't seen productivity gains, and he names three causes.

## Why gains haven't shown up

1. **Mindset.** An engineer's job is now getting the LLM to write better code faster. Each engineer is a team lead whose direct reports are models and agents. Practical steps include repo-level context files, shared skills and prompts, and treating repeated failures as bugs to fix. Human judgment, skepticism and accountability still matter, and maintainability, docs and tests stay important, though the right balance (for example, between test types) may shift as costs change.
2. **Stuck at "phase 1".** Using LLMs to speed up tasks inside existing workflows isn't enough. Phase 2 means examining the whole development lifecycle and removing bottlenecks (CI capacity, specs, approvals, design, code review), each of which exposes the next. He predicts most human code review will eventually end, and that some processes need redesign rather than optimization.
3. **Collective action.** Individuals can adopt LLMs alone, but retooling the company needs organization and funding Proton hasn't committed yet. It often doesn't know where its bottlenecks are because it hasn't built tools to find them.

## What's expected

Engineers own how work gets done, not just the output: keep optimizing and automating, share improvements, and escalate what they can't fix alone. Proton promises to organize and fund the phase 2 investments, with details to follow. The memo is a statement of direction and explicitly skips security, reliability and infrastructure specifics.
