---
title: "Artificial Analysis Index v5: 60% private tests, Opus 5.5 leads at 58"
slug: "artificial-analysis-index-v5-private-benchmarks"
date: 2026-10-28T08:03:06+0000
summary: "Artificial Analysis released Intelligence Index v5, the first version where most of the score (60%) comes from private test sets or held-out answers. Claude Opus 5.5 (max) leads at 58, ahead of GPT-6 Astra (57) and GPT-6.1 Sol (55), and the gap between open-weights and proprietary models widens from 3 points to 12."
source: "https://artificialanalysis.ai/articles/artificial-analysis-intelligence-index-v5"
source_title: "Announcing Artificial Analysis Intelligence Index v5"
source_author: "Artificial Analysis"
source_site: "Artificial Analysis"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50268868"
---

Artificial Analysis shipped Intelligence Index v5, its biggest upgrade since v4 in January. 60% of the index now rests on private test sets or held-out answers, to limit benchmark contamination. They also launched a separate Progress Index v1 that tracks capability over time on one continuous scale.

**What changed**
- New: FrontierCode 2.0 (Cognition's private benchmark, 132 real open-source repo tasks, graded on whether a maintainer would merge the agent's change) and Terminal-Bench-Science (70 agentic research tasks).
- Upgraded: HLE-Diamond (a refined 1,000-question Humanity's Last Exam), AA-Omniscience v1.1, and a private 40-question hard subset of CritPt v2.
- Removed: GDPval-AA v2.1, SciCode and AA-LCR v1.1.
- Weights: Knowledge Work, Coding, Science and General each count for 25%.

**Results**
- Claude Opus 5.5 (max) leads at 58, then GPT-6 Astra (57) and GPT-6.1 Sol (55). Anthropic and OpenAI hold the top five spots; Gemini 4 Argon (high) is next at 50.
- The open-weights gap grows from 3 to 12 points. Kimi K3.1 Pro (weights not yet released) is the top open model at 46. Among released weights, DeepSeek V4.1 Flash 1017 and GLM-5.3 lead at 32, below Claude Haiku 5.5 at 36.
- Cost per index task differs by more than 13x among the top 10 models: $0.96 for GPT-6.1 Terra versus $13.04 for Claude Fable 5.1.
- On FrontierCode 2.0, Opus 5.5 scores 46%; the best open-weights model scores 28%.
