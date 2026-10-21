---
title: "Vendor benchmark: Cloudflare Bot Management caught 9% of AI-agent sessions"
slug: "bot-detection-benchmark-ai-agents-2026"
date: 2026-10-21T01:03:00+0000
summary: "In a benchmark run by bot-detection vendor Proof of Human, five detection systems caught between 9% and 99% of automated sessions. Scripted browsers were caught far more often than AI agents. Treat the ranking with care: the winner is the authors' own product."
source: "https://research.poh.org/bot-benchmarking-2026/"
source_title: "Benchmarking Bot Detection Systems Against Modern AI Agents in 2026"
source_author: "Milena Rmus, Mayank Agrawal, Mathew Hardy"
source_site: "Proof of Human Research"
source_date: "2026-10-08"
hn_url: "https://news.ycombinator.com/item?id=50169506"
---

**Bottom line:** Proof of Human tested five bot-detection systems against five kinds of automation. Overall detection ranged from 99% down to 9%. The split that mattered was scripted browsers versus LLM-driven agents. Interactive challenges and browser-fingerprint checks did poorly against the agents. The authors sell the top-scoring product, so the ranking is not independent.

## Setup

- Each adversary completed a five-question survey 20 times per detection system, giving 89 to 100 usable sessions per system.
- Adversaries: plain Playwright Chromium, CloakBrowser (a stealth Chromium fork run through a residential proxy with humanized input), Browser Use (driving a browser with Claude Opus), Perplexity Comet, and xAI's Grok browsing agent.
- Systems: Proof of Human, Google reCAPTCHA v3, hCaptcha, Cloudflare Turnstile, and Cloudflare Bot Management.

## Results (share of automated sessions flagged)

- Proof of Human: 99%
- reCAPTCHA v3: 69.4%
- hCaptcha: 44.3%
- Turnstile: 42.1%
- Cloudflare Bot Management: 9.1%

## Patterns

- hCaptcha and Turnstile caught almost all scripted Chromium and CloakBrowser sessions but few agent sessions. The vision-capable agents solved hCaptcha's image grids, and Turnstile never even issued them a challenge.
- reCAPTCHA v3 showed the reverse pattern. It flagged most agent sessions and plain Chromium, but missed CloakBrowser entirely. The authors can't tell whether the fingerprint or the residential IP was responsible.
- Cloudflare Bot Management flagged few sessions of any kind, which the authors read as agents and stealth browsers presenting clean browser-level signals.
- Their conclusion is that behavioral signals (typing, mouse, scrolling) matter more as fingerprints get easier to fake.

## Caveats

The authors list several limits themselves: they used lower product tiers rather than enterprise ones, did not measure false positives on real humans, ran only a simple survey task, and the sample sizes differ slightly. A 99% detection rate means little without a false-positive rate. They also note that reCAPTCHA may improve with traffic history on a site.
