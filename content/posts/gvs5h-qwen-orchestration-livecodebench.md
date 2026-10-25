---
title: "Five-instance Qwen3.8-27B scaffold scores 92.4% on LiveCodeBench Hard"
slug: "gvs5h-qwen-orchestration-livecodebench"
date: 2026-10-25T05:02:39+0000
summary: "GVS5H is a training-free scaffold where fresh instances of one model decompose a problem and coordinate through a shared filesystem holding a plan, notes and current solution. On 100 recent hard LiveCodeBench problems the authors report Qwen3.8-27B rising from 66.8% to 92.4% pass@1, and Qwen3.8 Flash Next reaching 93.0% versus Fable 5's 90.4% at 9% of the cost. Results are self-reported."
source: "https://github.com/slee-persis/GVS5H"
source_title: "GVS5H: Five Qwen3.8-27B Models Match Claude Fable 5 on LiveCodeBench Hard"
source_author: "slee-persis"
source_site: "GitHub"
hn_url: "https://news.ycombinator.com/item?id=50228167"
---

**Bottom line:** The authors claim inference-time organization alone can bring small or cheap models to frontier coding accuracy: orchestrated Qwen3.8-27B (locally served, open weights) goes from 66.8% to 92.4% pass@1, and orchestrated Qwen3.8 Flash Next hits 93.0% against Fable 5's 90.4% at 9% of the cost.

- **Method:** "ledger-based zero-shot self-orchestration". No training; fresh instances of one model decompose the problem and coordinate through a shared filesystem with a plan, notes and the current solution.
- **Benchmark:** the 100 latest hard LiveCodeBench problems, across eleven open and closed-weight models, with gains up to 25.6 points on pinned backends.
- **Caveats:** gains aren't universal (some models are unchanged or worse), the Opus 5 run used an earlier scaffold via OpenRouter, and all numbers come from the authors' own paper and repo, not independent replication.
- **Why it works, per the authors:** transcript analysis attributes the gain to decomposition and persistent context.
- **Try it:** the repo runs with `uv` and `--engine multiagent` versus `--engine single` for the baseline.
