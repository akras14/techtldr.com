---
title: "Pi as the last coding-agent harness: one engineer's case for a minimal engine"
slug: "pi-last-harness-engine-bet"
date: 2026-11-01T16:02:12+0000
summary: "Destiner argues that Pi, the open-source coding-agent harness, is the one most likely to become the last widely used one, because it is vendor-agnostic, minimal and extensible. The post is an opinion bet, not a benchmark: AI coding workflows will keep churning, but stable, unopinionated tooling that avoids lock-in will win."
source: "https://destiner.io/blog/post/pi-is-the-last-harness/"
source_title: "Pi is the Last Harness (Engine)"
source_author: "Destiner"
source_site: "Destiner's Notes"
source_date: "2026-11-01"
hn_url: "https://news.ycombinator.com/item?id=50326025"
---

**Bottom line:** The author bets that Pi will become the last widespread agentic coding harness, since it is the only one that is open source, vendor-agnostic, extensible and minimal enough to impose no opinions. This is a personal prediction, not measured evidence.

- **What changes and what stays:** Practices like approving every tool call, plan mode and human code review keep shifting with model progress. Docs, test coverage, static checks, modular code and version-control hygiene keep helping models just as they help people. Harnesses will stay "text in, text out" with bash and MCP tool calling, and users will value stability and no lock-in.
- **Why Pi:** Minimalism avoids bloat that hurts frontier models and spares the tool from unshipping obsolete features. Extensibility matters more, since no two workflows match (for example a custom `/usage` command across providers). Switching providers without switching apps is reason enough on its own.
- **Why it should last:** Pi has been around a year and has arguably the largest developer community among harnesses. The author can't name a 10x improvement over it. Missing features such as cloud agents, dynamic workflows, model routing and better plugins could arrive natively or as plugins.
- **"Engine" over "harness":** Prompts and tools will converge. The lasting part is the engine: a unified provider interface, compaction, automatic retries and transcript handling, with users tailoring the rest.
- **What to watch:** Orbs/Amp (ephemeral per-workload VMs), the DeepSeek harness (everything is a plugin), and omp², whose release breakdown argues harness design is still a young space.
