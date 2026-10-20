---
title: "Agent-written monitoring specs beat pure agent loops for regression detection"
slug: "agentic-monitoring"
date: 2026-10-20T00:04:00+0000
summary: "Ben Congdon argues that LLM agents will take over production monitoring. In his prototype, an agent proposes a monitoring plan for each release or config push, a deterministic scaffold executes it, and a second agent investigates triggered monitors. He found this cleaner and much cheaper in tokens than a pure agent loop, and says it caught regressions in systems he knew nothing about."
source: "https://benjamincongdon.me/blog/2026/10/19/Agentic-Monitoring/"
source_title: "Agentic Monitoring"
source_author: "Ben Congdon"
source_site: "benjamincongdon.me"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50153225"
---

**Bottom line:** Congdon, prompted by an Anthropic post describing Slack-channel "superfan" Claudes that watch single components, says agent-driven monitoring is the obvious next step for operations. He thinks it will soon be reckless not to have it, and that starting early is an unfair advantage.

**His case study: change-point monitoring.** Releases and config pushes cause most outages, but human-run systems struggle to monitor them. Targeting well-run teams adds little safety, while opting in every service on generic health metrics is too noisy. Agents sidestep the precision/latency tradeoff because they will run many queries, write per-change queries on the fly and scan logs continuously.

**Design finding.** A pure agent loop was not the best shape. Letting the agent write a monitoring spec that a deterministic system then runs gave cleaner signals with far fewer tokens. The remaining "secret sauce" is noise-reduction heuristics, prompts that produce good specs, and a labeled eval set.

**Why he thinks agents suit the job**
- They watch everything 24/7 and can fan out parallel investigations.
- They read code and digest logs and metrics faster than people.
- They can be diligent in ways no one would pay a person for, such as tracking every commit.
- They can propose performance work. He reports a few Opus-class sessions finding a 50% heap saving and 30% latency cut in a hot-path library humans had already tuned.
- Cost is low: he estimates a moderately large company could monitor its whole fleet with small models for around $100/day.

Write-path remediation (rollbacks, resizing, root-cause fixes) is possible with guardrails, but most of the post concerns read-only monitoring. These are the author's claims from a prototype, not measured results.
