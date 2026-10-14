---
title: "Encoder-only classifier halves latency of a 2B decoder, but scales worse with prompt length"
slug: "strands-decider-encoder-vs-decoder"
date: 2026-10-14T20:02:29+0000
summary: "Marc Brooker tested whether strands-decider, a small model that makes agent-policy decisions, should be an encoder like BERT instead of a modified decoder LLM. The best encoder-only variant (e1b) matched the 2B Qwen-based decoder (v19) on accuracy and calibration and cut median latency from about 65 ms to about 35 ms. But no design was a clear winner: the encoder gets slower faster as prompts grow, and the experiment is confounded by differing architectures and base models."
source: "https://brooker.co.za/blog/2026/10/04/encoders.html"
source_title: "Strands Decider: Why Not an Encoder?"
source_author: "Marc Brooker"
source_site: "brooker.co.za"
source_date: "2026-10-04"
hn_url: "https://news.ycombinator.com/item?id=50088447"
---

**Bottom line:** An encoder-only version of strands-decider-2B roughly matched the decoder-based design on accuracy and halved median latency, but it scales worse on long prompts, so Brooker calls the comparison inconclusive and is not switching.

**What was tested.** Five designs: v19 (the shipped Qwen3.5-based decoder), two T5Gemma 2 encoder-decoders (1B+1B and 4B+4B), and two encoder-only variants built from T5Gemma's 2.6B encoder. e1a is a plain encoder; e1b adds a masked state cache so several questions can be evaluated in parallel and the state reused. Frozen ModernBERT and ettin-encoder-1b did worse per parameter and were dropped.

**Accuracy.** The 4B+4B encoder-decoder (b2) beat v19 on JevBench accuracy and Brier score, but it has about 8B parameters against v19's 2B. The fair comparison, e1b vs v19, was a wash: slightly better JevBench accuracy within v19's run-to-run variance, slightly worse calibration, and a tie on JF100 calibration.

**Latency.** Encoder-only models had a median of about 35 ms against v19's 65 ms; encoder-decoders were slowest. e1b wins at short prompts but scales worse as they grow, since it has 26 full-attention layers with quadratic cost while 75% of v19's layers are linear-cost Gated DeltaNet. Whether that matters depends on the workload; the eval sets skew short.

**Curiosity.** The unmasked encoder e1a beat v19 on seven of nine eval sets but did notably worse on JevBench (153 vs 168) and JF100 (158 vs 162), suggesting it generalizes worse than the masked design.

**Caveats.** Brooker says he is not an expert model developer, and the experiment changes architecture, base model and instruction tuning at once. The code lives on a branch and isn't merged into mainline.
