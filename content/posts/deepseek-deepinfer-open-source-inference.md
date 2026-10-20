---
title: "DeepSeek open-sources DeepInfer, its production inference engine"
slug: "deepseek-deepinfer-open-source-inference"
date: 2026-10-20T10:02:37+0000
summary: "DeepSeek released DeepInfer, the inference framework behind its production services, under Apache 2.0. This is an offline-inference release that supports only DeepSeek-V4.1-Flash, on NVIDIA SM100 GPUs and Huawei Ascend 950 NPUs, and it leaves out the KV cache system, prefill/decode disaggregation and load balancing a full serving stack needs."
source: "https://github.com/deepseek-ai/DeepInfer"
source_title: "deepseek-ai/DeepInfer"
source_author: "DeepSeek-AI"
source_site: "GitHub"
hn_url: "https://news.ycombinator.com/item?id=50163277"
---

DeepSeek has open-sourced DeepInfer, the inference framework it says powers its large-scale production services, under the Apache 2.0 license. The release is partial: it provides offline inference for a single model, DeepSeek-V4.1-Flash, and omits pieces you would need for a complete serving system.

**What's included**
- Optimized kernels (FlashMLA, DeepGEMM, DeepEP, TileKernels and custom TileLang kernels), plus the model and sampler runtime.
- Parallelism: attention data parallelism with balanced context parallelism, and MoE expert parallelism with load balancing (EPLB).
- Faster decoding through draft tokens with confidence-scheduled verification, and a bounded-replay trick that cuts decoder prefill work.
- An asynchronous, zero-overhead batch scheduler that overlaps CPU scheduling with device execution.
- Batch-invariant inference and deterministic sampling (NVIDIA only), so identical inputs give identical outputs regardless of batching.
- Structured outputs constrained by regex, JSON schema or EBNF grammar.

**What's not included.** The README says a complete service also needs a KV cache system, prefill/decode disaggregation (most of the engine code for it is already there), and load balancing across data-parallel replicas. None of these ship in this release.

**Hardware and setup.** It supports NVIDIA SM100 GPUs and Huawei Ascend 950 NPUs, and needs Linux, GCC 13+, Python 3.12, and CUDA 13 or CANN 9.2. The published benchmarks used 4 SM100 GPUs for prefill and 64 for decode. DeepSeek notes DeepInfer was originally built mainly on vLLM, with ideas from SGLang and TensorRT-LLM.
