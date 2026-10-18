---
title: "vLLM's agent-written Kimi K3 decode path is 1.6-1.9x faster on 16 GB300s"
slug: "vllm-kimi-k3-agent-optimized-decode-1-7x"
date: 2026-10-18T23:02:24+0000
summary: "vLLM contributors used coding agents to build a fused CuTe DSL decode path for Kimi K3 on NVIDIA GB300s, reaching 842 tokens/s at batch 1 on 16 GPUs with speculative decoding (1.73x stock vLLM) and 1.62-1.92x without it on TP16. The larger point is process: agent-written kernels outrun line-by-line review, so vLLM proposes a fast-track model-optimization path built on isolation, bounded scope, systematic validation and broad benchmarks."
source: "https://vllm.ai/blog/2026-10-16-kimi-k3-megakernel"
source_title: "Hyper Optimizing Kimi K3 in vLLM: Moving Fast and Responsibly"
source_author: "Benjamin Chislett (NVIDIA)"
source_site: "vLLM Blog"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50140784"
---

vLLM contributors used coding agents to build a unified CuTe DSL decode path for Kimi K3 on NVIDIA GB300 GPUs. It reaches 842 tokens/s at batch 1 on 16 GPUs with DSpark speculative decoding (synthetic acceptance length 6), 1.73x stock vLLM. Without speculation, TP16 is 1.62-1.92x faster across batches 1-64 and TP8 is 1.31-1.58x. The post's second theme is that agent-optimized code is outpacing human review, so vLLM proposes a "fast-track" process for such contributions.

**What was optimized**
- Kernels are tuned per batch size (small-matrix kernels at batch 1-2, split-K at larger batches), covering attention, MoE and communication. Per layer, kernel count drops from 23 to 13 and time from 130 to 69 microseconds.
- Attention output projection is fused with its all-reduce over NVLink, and MoE routing work is split across tensor-parallel ranks. The shared expert runs on a side stream, and a side-stream kernel prefetches upcoming weights into cache.
- On AgentX, a replay of real coding-agent traffic, P90 interactivity at concurrency 1 rises to 403.2 from 223.7 tokens/s/user (1.8x). That compares 16 GPUs against an earlier 8-GPU result.

**The process argument**
Reviewing every line of every kernel across every configuration doesn't scale. vLLM's four principles:
1. Isolate changes inside the model's code, with a traceable fallback to the standard path.
2. Bound the scope to specific hardware and batch ranges, but don't drop production features such as full context, sampling or structured output.
3. Validate correctness with both kernel tests and end-to-end evals. During K3 work, an audit found the compiler moving activation reads ahead of a synchronization wait. Long-context and long-generation evals (MRCR, AIME25) also exposed numerical problems that mock-input kernel tests missed.
4. Check performance broadly, with real prompts for MoE and decode separated from mixed prefill/decode.

They also propose a Bring-Your-Own-GPU evaluation RFC so contributors can run standard evals on their own hardware. A similar opt-in design is planned for DeepSeek-V4.1-Flash. The benchmark numbers come from vLLM's own controlled setup, including a synthetic acceptance length.
