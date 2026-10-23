---
title: "ThinkingCap cuts Gemma-4-26B-A4B thinking tokens 39% for 0.15pp accuracy"
slug: "thinkingcap-gemma-4-26b-less-thinking"
date: 2026-10-23T18:03:01+0000
summary: "BottleCap AI fine-tuned Google's Gemma-4-26B-A4B to think less. Across twelve benchmarks it uses 38.9% fewer thinking tokens on average (up to 69.1%) for a 0.15pp average accuracy drop, and loops less often (0.8% vs 3.6%). The one real cost is AA-LCR, down 4.0pp. It is a drop-in replacement on vLLM."
source: "https://bottlecapai.com/post/thinkingcap-gemma-4-26b/"
source_title: "ThinkingCap: Gemma-4-26B-A4B — up to 69% and on average 39% less thinking, at unchanged accuracy"
source_author: "BottleCap AI"
source_site: "bottlecapai.com"
source_date: "2026-10-23"
hn_url: "https://news.ycombinator.com/item?id=50209907"
---

**Bottom line:** BottleCap AI's fifth ThinkingCap model shortens the reasoning of Google's Gemma-4-26B-A4B (a sparse MoE with about 4B active parameters) with almost no accuracy loss. These are the vendor's own measurements, run against the original model on the same harness and sampling settings.

- **Tokens:** mean thinking tokens fall from 8,773 to 5,398 (38.9% average reduction across twelve benchmarks). The deepest cuts are LongBench v2 (-69.1%) and AA-LCR (-59.3%); the shallowest are IFBench (-13.7%) and tau2-bench (-20.8%).
- **Accuracy:** 75.18% to 75.03% on average. Eight of twelve changes are within run-to-run noise. AA-LCR is the clear cost at -4.00pp; GPQA-Diamond is -0.96pp.
- **Looping:** responses that loop drop from 3.64% to 0.76%.
- **Use:** same sampling (thinking on, temperature 1.0, top_p 0.95) and same vLLM serving stack. A paid PRO version claims up to 87.6% fewer thinking tokens.
