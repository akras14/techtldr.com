---
title: "Text watermarking makes repeated AI responses more alike, despite 'no quality impact' claims"
slug: "ai-watermarking-reduces-response-diversity"
date: 2026-11-01T02:01:55+0000
summary: "Anthropic says watermarking doesn't affect Claude's output quality, but that claim rests on single responses. The SynthID-Text paper it builds on admits a diversity/detectability trade-off: watermarked responses to the same prompt are more similar to each other, which can lower overall quality in retry-heavy or best-of-N use."
source: "https://blog.keyvan.net/p/ai-text-watermarking-and-quality"
source_title: "AI text watermarking and quality loss"
source_author: "Keyvan"
source_site: "Keyvan's blog"
source_date: "2026-08-24"
hn_url: "https://news.ycombinator.com/item?id=50320599"
---

**Bottom line:** Anthropic claims watermarking "does not impact the quality of Claude's output," but the evidence behind that is per-response. The author argues that the SynthID-Text paper's own supplement concedes watermarking reduces diversity across multiple responses to the same prompt, and that this can lower overall quality.

- **Context:** After the EU AI Act's transparency guidelines, major labs are expected to watermark text output. John Gruber objected that any quality sacrifice for provenance is unacceptable; others replied that watermarking merely swaps random numbers.
- **The caveat:** The 56-page supplement describes a "diversity/detectability trade-off": watermarked responses to one prompt are more similar than unwatermarked ones, which "could lower the overall quality of a system which generates many responses then selects the best one."
- **Practical effects:** Hitting retry yields near-repeats; consensus-based confidence checks (sampling many answers) become less trustworthy because samples are no longer independent.
- **Evidence:** A reproduction of SynthID-Text on an open model by Natzir Turrado found four of five watermarked answers opened near-identically, versus more varied openings without the watermark.
- **Open gap:** The author could not find human evaluations of the cross-response effect, so the "preserves quality" results may not cover the most affected behavior.
