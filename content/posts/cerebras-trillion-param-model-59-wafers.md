---
title: "Cerebras serves a 3T-parameter model from SRAM across 59 wafers"
slug: "cerebras-trillion-param-model-59-wafers"
date: 2026-10-25T04:02:24+0000
summary: "Cerebras says it serves multi-trillion-parameter models at 1,000-2,000+ tokens/s by keeping all weights in on-chip SRAM and splitting the model layer-by-layer across wafers in a pipeline. Its worked example, the open ~3T-parameter Kimi K3 at 4-bit weights, needs 59 WSE-3T wafers (44 GB each): 24 dense-attention layers get one wafer apiece, and the 69 linear-attention layers fit two per wafer."
source: "https://www.cerebras.ai/blog/how-we-deploy-trillion-parameter-frontier-models-on-cerebras"
source_title: "How we deploy Trillion-parameter frontier models on Cerebras"
source_author: "Sarah Chieng"
source_site: "Cerebras"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50227356"
---

**Bottom line:** Cerebras serves trillion-parameter models by never moving weights. Each layer sits in a wafer's SRAM, and only activations travel between wafers. The company claims 1,000-2,000+ tokens/s this way.

**The worked example (Kimi K3, ~3T parameters, 93 layers):**

- At 4-bit quantization the weights are about 1.5 TB, or roughly 16 GB per layer. An 8-bit KV cache and an assumed 256k average request size are used.
- A WSE-3T wafer has 44 GB of SRAM. Dense (full-attention) layers need KV-cache room, so each gets a wafer to itself: 24 wafers.
- Linear-attention layers keep only a small fixed-size state per request, so two fit per wafer: 69 layers need 35 wafers. Total: 59 wafers.

**How it runs:**

- The model is a pipeline: wafer 1 runs the first layers and hands activations to wafer 2, and so on. Hops use RDMA over Ethernet at about 2 microseconds each.
- To avoid idle wafers, many requests (or micro-batches) are in flight at once, one per stage. Smaller micro-batches cut per-request latency; larger ones raise throughput.
- Concurrency is capped by the SRAM left over for KV cache on each dense-layer wafer (44 GB minus one 16 GB layer, divided by the per-request cache).

Cerebras argues the meaningful metric is tokens per second per megawatt at a fixed generation speed, not batch size. The post gives no measured throughput for this exact setup, and its speed claims come with the usual vendor caveat on comparisons with GPUs.
