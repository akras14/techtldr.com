---
title: "Mercury 3 diffusion LLM runs at 1,068 tokens/s for $0.20/$0.70 per 1M"
slug: "mercury-3-diffusion-llm-1000-tokens-per-second"
date: 2026-10-19T18:03:01+0000
summary: "Inception's Mercury 3 is a diffusion-based LLM it says matches speed-optimized frontier models like GPT-6 Luna and Claude Haiku 5.5 while running 4-8x faster, at 1,068 tokens/s on NVIDIA GPUs. It has a 1M-token context and costs $0.20 per 1M input and $0.70 per 1M output tokens (half price at launch)."
source: "https://www.inceptionlabs.ai/blog/introducing-mercury-3"
source_title: "Diffusion at Scale: Introducing Mercury 3"
source_author: "Inception Team"
source_site: "inceptionlabs.ai"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50155245"
---

**Bottom line:** Inception claims Mercury 3 shows text diffusion scales: a model over 40x larger than Mercury 1 that is still fast, with quality on par with or better than GPT-6 Luna and Claude Haiku 5.5 on its benchmarks, at 4-8x the speed.

- **Speed and context:** 1,068 tokens per second on NVIDIA GPUs, with a 1M-token context (up from 260K in Mercury 2.5).
- **Price:** $0.20 per 1M input and $0.70 per 1M output tokens; launch pricing is 50% off ($0.10/$0.35). Cached input tokens are 90% off.
- **Scaling claim:** Inception says intelligence rose 3x from Mercury 1 (mini) to Mercury 3 across standard benchmarks, and that autoregressive-style scaling laws hold for diffusion.
- **Agent test:** In TinyFish's production web-agent harness, Mercury 3 completed 50% of 50 live Mind2Web tasks, more than GPT-6.1-Sol, using similar token volume and step counts at lower per-token cost. This is a partner-run, small-sample result.
- **Early users:** Partners cite browser agents, voice and chat support, tutoring and search agents, where latency compounds across steps.
- **Related products:** Mercury Voice (low time-to-first-token) and Mercury Decide (high-volume classification).
- **Availability:** Inception API, OpenRouter, Baseten and Vercel; OpenAI API compatible.

All figures are the vendor's own, not independent evaluations.
