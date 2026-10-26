---
title: "Non-English AI users pay 1.5-5x more tokens among top 20 languages, up to 17x across 200"
slug: "tokenizer-tax-non-english"
date: 2026-10-26T22:02:43+0000
summary: "A study by the Coalition for Language Equity in AI Research finds that tokenizers use far more tokens for some languages, a hidden \"tokenizer tax\". Among the 20 most popular internet languages the costliest uses 1.5 to 5 times as many tokens as the cheapest; across 200 languages the gap in one model grew to 17 times. That means higher prices, more latency and less usable context for speakers of those languages."
source: "https://restofworld.org/2026/ai-tokenizer-tax-non-english-languages-cost-inequity/"
source_title: "AI models cost more when you don't use English"
source_author: "Ananya Bhattacharya"
source_site: "Rest of World"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50250910"
---

**Bottom line:** token counts for the same text vary widely by language, so users of some languages pay more per prompt, wait longer and fit less into the context window.

- The cost gap tracks script as much as language rarity. Latin-script languages benefit, even small ones like Asturian. Burmese, Tibetan, Tifinagh, Khmer, Lao and Ol Chiki were consistently worst off across nearly all models tested.
- Beyond price, higher token counts mean more latency and less effective context, for example a tutoring app's history fills up faster.
- Sources quoted say falling token prices will ease direct cost, but fixing the gap requires labs to design tokenizers for these scripts and not just add better data.
- One investor suggests labs publish a language-by-language scorecard of the cost to complete comparable tasks.
