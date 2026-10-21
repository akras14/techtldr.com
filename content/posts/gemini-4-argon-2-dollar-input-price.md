---
title: "Gemini 4 Argon ships at $2/$10 per million tokens, topping the Vals Index"
slug: "gemini-4-argon-2-dollar-input-price"
date: 2026-10-21T05:02:47+0000
summary: "Google released Gemini 4 Argon to everyone, via the API (AI Studio and Gemini platform), Antigravity for AI Ultra, and the Gemini app and AI Mode for Ultra and Pro subscribers. It launches at an introductory $2 per million input tokens and $10 per million output tokens, with cached input 95% off, and Google says it ranks #1 on the Vals Index of finance, coding, legal and tax work. The pitch is long-running agentic engineering: Google reports Antigravity teams submitting 35% more agentic code with nearly a third fewer rollbacks."
source: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-argon/"
source_title: "Gemini 4 Argon: Built to solve your hardest problems"
source_author: "Koray Kavukcuoglu"
source_site: "Google"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50168983"
---

Google's Gemini 4 Argon is now generally available. Developers get it through the API, Google AI Ultra subscribers get it in the Antigravity coding environment, and Ultra and Pro subscribers get it in the Gemini app and AI Mode in Search. Pricing is introductory: $2 per million input tokens and $10 per million output tokens, with cached input tokens 95% cheaper.

## What Google claims

- **Benchmarks:** #1 on the Vals Index, which covers finance, coding, legal and tax work. These are Google's own reported results.
- **Internal engineering use:** engineers using Argon in Antigravity increased agentic code submissions by more than 35% and cut rollbacks by nearly a third.
- **Chrome refactor:** Argon helped separate core webpage handling from a roughly 6 million line legacy folder of browser features in Chromium's 40 million line codebase, across hundreds of files, in weeks rather than months.
- **ADK optimization:** working overnight from a one-paragraph goal and a skills rulebook, it cut in-memory session prep time by 40% in long sessions and sped up cold imports by 21% in google/adk-python.
- **Other speedups:** xTB, a quantum chemistry package, up to 2.3x faster at predicting molecular structures, and a 2.85x end-to-end vLLM inference speedup for Gemma 4 E2B on an A100 (about 37,900 to over 108,000 tokens per second) with token parity.
- **Other domains:** cybersecurity (finding and patching vulnerabilities), plus 3D and visual coding demos driven by multi-agent loops.

Most of the evidence is Google's own projects and early-access customers, so independent results will matter more than the demos.
