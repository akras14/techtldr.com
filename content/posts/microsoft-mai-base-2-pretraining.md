---
title: "Microsoft's MAI-Base-2: 2.8T-parameter MoE trained on 16k GB200s in 38 days"
slug: "microsoft-mai-base-2-pretraining"
date: 2026-10-15T20:02:13+0000
summary: "Microsoft AI says its new MAI-Base-2, a mixture-of-experts model with 80B active and 2.8T total parameters, beats every open-weight model it tested on held-out evaluations and is 4.2-12.3x more pre-training FLOPs-efficient than DeepSeek-V4-Pro-Base. It was trained on 16k GB200 GPUs over 38 days at 21% MFU and 89% goodput, and is now in post-training. These are Microsoft's own benchmark claims."
source: "https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/"
source_title: "Frontier Pre-training at Microsoft AI"
source_author: "Microsoft AI"
source_site: "microsoft.ai"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50105791"
---

**Bottom line:** Microsoft AI has finished pre-training MAI-Base-2, an MoE foundation model with 80B active and 2.8T total parameters. It reports lower bits-per-byte than every open-weight model it compared (DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B, MiMo-V2.6-Pro-RL) and estimates its recipe is 4.2-12.3x more FLOPs-efficient than DeepSeek-V4-Pro-Base, and 6.3-12.7x better than its own June MAI-Base-1. It reinvested the savings in a bigger model. Benchmarks are self-reported; for most competitors only post-trained checkpoints were available, which Microsoft flags as a caveat.

**Run details:** 16k GB200 GPUs, 38 days (August-September 2026), average MFU 21%, goodput 89%. Evals are held-out text, Microsoft's own codebase for code, and vendor-commissioned STEM, math and Q&A sets, decontaminated against training data.

**What it credits:**
- Scaling ladders with "source scaling": smaller rungs restrict unique data so repetition matches the full run, keeping results transferable.
- Hyperparameter scaling laws: LR x WD ~ 17.04 / steps, plus fitted laws for LR and dropout.
- Model-infrastructure co-design: improved MoE and Muon implementations and activation gradient clipping.
- More data, including vision data.

**Business angle:** Microsoft says pre-training uses organic data with no distillation from other models, which it pitches as clean provenance so it can license weights to partners via Foundry. MAI-Base-2 is currently in post-training.
