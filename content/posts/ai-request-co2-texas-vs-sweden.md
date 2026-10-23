---
title: "Same AI request emits 8.5x more CO₂ in Texas than in Sweden"
slug: "ai-request-co2-texas-vs-sweden"
date: 2026-10-23T08:04:39+0000
summary: "Swedish inference provider Berget AI released an open-source (MIT) method for computing CO₂ per individual AI request, built with SEI, WattTime and Klimatkollen. For one typical Kimi K3 request on identical hardware it estimates 0.4 g in Sweden versus 3.2 g in Texas, a gap driven almost entirely by grid carbon intensity."
source: "https://berget.ai/en/blog/per-request-co2-press-release"
source_title: "New open-source method shows the same AI request emits 8.5 times more CO₂ in Texas than in Sweden"
source_author: "Berget Team"
source_site: "Berget AI"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50205229"
---

Swedish inference provider Berget AI released an open-source (MIT) method for computing CO₂ per individual AI request, built with SEI, WattTime and Klimatkollen. For one typical Kimi K3 request on identical hardware it estimates 0.4 g in Sweden versus 3.2 g in Texas, a gap driven almost entirely by grid carbon intensity.

## What it measures

- Per-request figures: about 0.4 g (Sweden), 1.6 g (Ireland), 2.5 g (Germany), 3.2 g (Texas). The press release was corrected after a source review: it first claimed ten-fold and now says 8.5x.
- Results also depend on the model, time of day and prompt caching, so each request is computed separately rather than averaged.
- Location-based (average grid mix) and includes embodied hardware emissions, about 1,000 kg CO₂ per data-centre GPU. In Sweden, 89% of a request's footprint comes from hardware manufacturing.
- Training is excluded. Water and chip-manufacturing energy are not yet covered.
- Follows the Green Software Foundation's SCI for AI spec. A calculator at co2.berget.ai applies it to any model and region.

## Why it matters

Berget's pitch is that open-weight models (29% of tokens on Vercel's AI Gateway in June 2026, up from 11% in April) can run in any data centre, so buyers can pick where. That only works if providers publish per-request numbers. Marginal-emissions estimates are coming next month and may change Sweden's figure significantly. This is a vendor press release, so treat the comparisons as the vendor's own estimates.
