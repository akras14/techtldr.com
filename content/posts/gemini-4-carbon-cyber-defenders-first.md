---
title: "Gemini 4 Carbon scores 88.2% on SEC-bench Pro, limited to cyber defenders"
slug: "gemini-4-carbon-cyber-defenders-first"
date: 2026-10-28T18:02:38+0000
summary: "Google announced Gemini 4 Carbon, two weeks after Argon, and says it ranks #1 on the Artificial Analysis Intelligence Index v5.0 (59) with state-of-the-art cybersecurity results: 88.2% on SEC-bench Pro (finding vulnerabilities), 54.8% on ExploitGym (exploitation) and 71.0% on CWE-bench (patching). It is not generally available: access starts with a subset of cyber defenders through the Fairwind Program while Google takes part in the U.S. government's voluntary pre-release testing. Pricing will match Argon's introductory $2/$10 per million tokens."
source: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-carbon/"
source_title: "Gemini 4 Carbon: our next frontier model"
source_author: "Koray Kavukcuoglu"
source_site: "Google"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50276792"
---
Google announced Gemini 4 Carbon, its strongest model, two weeks after Gemini 4 Argon. It is a staged release: Carbon goes first to a subset of cyber defenders and then to more trusted partners through the Fairwind Program. Google says it will reach developers, businesses and consumers "as soon as possible" once guardrails are tuned, and that it is taking part in the U.S. government's voluntary pre-release model testing. Introductory pricing matches Argon at $2 per million input tokens and $10 per million output tokens, with cached input 95% cheaper. After the introductory period it rises to $4 and $20.

## What Google claims

- **Overall:** #1 on the Artificial Analysis Intelligence Index v5.0 at 59, and #4 on WebDev Arena (1769, up 89 Elo).
- **Cybersecurity:** 88.2% on SEC-bench Pro (finding vulnerabilities), 54.8% on ExploitGym (exploitation) and 71.0% on CWE-bench (patching), all evaluated without guardrails. Internally, secure-code generation succeeds more than 2x as often as with Argon, and the Chrome team reported patches of much higher quality than another lab's frontier model (over 10x better, by Google's account).
- **Long-horizon work:** Google says internal teams have seen Carbon run for days on root-causing and patching subtle bugs in large codebases and low-level systems.
- **Alignment:** cheating on Terminal-Bench 4.0 cut to 0.0% (0.4% of attempts), high-severity misalignment alerts down almost 80% in live internal coding, unsupported claims roughly halved versus Argon, overreach down 26% and under-service down 29%.
- **Prompt injection:** 0.0% attack success on Gray Swan's indirect prompt injection benchmark, versus 0.7% for Argon.

These are Google's own figures and mostly internal tests. The model isn't broadly available, so independent verification will have to wait.
