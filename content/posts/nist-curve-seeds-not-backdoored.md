---
title: "Anthropic researchers reverse-engineer the NIST curve seeds, finding no backdoor"
slug: "nist-curve-seeds-not-backdoored"
date: 2026-10-20T04:02:30+0000
summary: "Researchers at Anthropic recovered how the NSA-supplied seeds for the NIST elliptic curves were generated and showed they were not rigged with a backdoor. They claimed the bounty Filippo Valsorda announced in 2023, and Valsorda verified the result. Jerry Solinas had generated the seeds in good faith, but his memory had faded; the process turns out to involve a date, not just a phrase."
source: "https://www.johndcook.com/blog/2026/10/17/nist-curves-exonorated/"
source_title: "Cloud of suspicion lifted from NIST curves"
source_author: "John D. Cook"
source_site: "johndcook.com"
source_date: "2026-10-17"
hn_url: "https://news.ycombinator.com/item?id=50156132"
---

**Bottom line:** Anthropic researchers reverse-engineered where the seeds for the NIST elliptic curves (P-192 through P-521) came from and showed they were not engineered to hide a backdoor. They claimed the bounty Filippo Valsorda announced in October 2023, and Valsorda verified the result and paid it out.

## Background

- NIST published the curves in 2000 with seeds supplied by the NSA. The curves have the form y² = x³ − 3x + b, and the worry was that the choice of `b` could have been engineered to make discrete logarithms easy for someone with inside knowledge.
- Suspicion grew after Snowden, and because the NSA really did backdoor the Dual_EC_DRBG random number generator.
- In 2014 Daniel Bernstein asked NSA's Jerry Solinas where the seeds came from. Solinas said they were produced by hashing an English message, but he couldn't remember the message or the exact procedure.

## What changed

- The reconstruction shows Solinas derived the seeds in good faith; his memory had simply faded. The process used a date as part of the input, not only a phrase.
- Valsorda's take: this completes the "rehabilitation arc" of the NIST curves, which he would now personally recommend over Curve25519/Ed25519/X25519, just as they are being retired in favor of post-quantum signatures.

## Caveat from the author

Cook notes this doesn't mean LLMs threaten encryption. Finding the seeds took a lot of brute force, with some cleverness in proposing candidates to check. That is a different problem from finding weaknesses in cryptographic primitives.
