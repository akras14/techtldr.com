---
title: "Speculative decoding does more work, but spends idle GPU compute"
slug: "speculative-decoding-memory-bound"
date: 2026-10-20T12:03:10+0000
summary: "Speculative decoding isn't fast because it does less work. It does more. It's fast because single-sequence decoding is memory bound, so the GPU has spare compute, and verifying several draft tokens in one forward pass uses that spare compute to advance one sequence further."
source: "https://jbarrow.ai/2026-10-08-why-is-speculative-decoding-fast/"
source_title: "Why is Speculative Decoding Fast?"
source_author: "Joe Barrow"
source_site: "jbarrow.ai"
source_date: "2026-10-08"
hn_url: "https://news.ycombinator.com/item?id=50152539"
---

Speculative decoding is fast not because verifying a draft takes fewer FLOPs than generating the tokens, but because it changes the kind of work the GPU does. Total FLOPs actually go up for the same output sequence.

**Why there's free compute.** Decoding one token at a time is memory bound: every step streams all the active parameters of the model (the post's example is 27B for Qwen3.5-27B) from global memory, and the tensor units sit partly idle. Decoding a large batch is compute bound instead. At small batch sizes, adding a second sequence costs no extra time per token.

**Breadth vs. depth.** If the optimal number of tokens per forward pass is, say, 5, you can fill it with 5 sequences each advancing one token (breadth), or with 5 draft tokens from one sequence (depth). The GPU does about the same work either way. Speculation spends the spare compute on making one sequence progress a lot.

**Why it matters in production.** Once the batch is already near the compute-bound point, speculating just adds wasted work, and every rejected draft token is pure waste. That's why vLLM has `--speculative-disable-by-batch-size`. It also motivates research on varying speculation depth: deep when the batch is small and compute is free, shallow or per-sequence adaptive when load is high, the idea behind dSpark.
