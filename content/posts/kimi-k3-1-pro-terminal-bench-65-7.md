---
title: "Kimi K3.1 Pro scores 65.7% on Terminal-Bench 4.0, open weights due Nov 4"
slug: "kimi-k3-1-pro-terminal-bench-65-7"
date: 2026-10-23T18:03:01+0000
summary: "Moonshot released Kimi K3.1 Pro, a post-trained K3 (3T-class, 1M-token context) with much more RL compute. It reports 65.7% on Terminal-Bench 4.0, ahead of GPT-6 Astra (58.2%) and just behind Claude Opus 5.5 (66.4%). A 2-3x faster Highspeed variant ships too, and full weights are promised by Nov 4."
source: "https://www.kimi.ai/blog/kimi-k3-1-pro"
source_title: "Kimi K3.1 Pro Tech Blog: Advancing Open Intelligence"
source_author: "Moonshot AI"
source_site: "kimi.ai"
hn_url: "https://news.ycombinator.com/item?id=50212065"
---

**Bottom line:** Moonshot's Kimi K3.1 Pro is the first model in the K3.1 series, a post-trained version of Kimi K3 with substantially more RL compute. The company calls it its most capable 3T-class open-weight model, with a 1-million-token context window. All benchmark numbers below are the vendor's own.

- **Coding:** 65.7% on Terminal-Bench 4.0, ahead of GPT-6 Astra (58.2%) and narrowly behind Claude Opus 5.5 (66.4%). On Moonshot's internal long-horizon Kimi Code Bench 3.0 it scores 61.2%, up from K3's 55.95%.
- **3D and CAD:** 92.7% on BenchCAD with tools, versus 96.2% for Opus 5.5 and 95.9% for GPT-6 Astra. It can write scripts for Blender and CAD tools directly.
- **Finance:** state-of-the-art on OfficeQA Pro v2 (64.4), plus gains on internal finance benchmarks.
- **Long-horizon demo:** over 167 missions it iterated on a human walking simulator, found 36 errors in shoulder-muscle moment-arm calculations, and got the simulated body walking for 11.2 seconds.
- **Availability:** K3.1 Pro and a Highspeed variant (2-3x faster) are live on Kimi.ai, Kimi Work, Kimi Code and the API. Full weights are promised by Nov 4, so the open-weight claim is not yet verifiable.
