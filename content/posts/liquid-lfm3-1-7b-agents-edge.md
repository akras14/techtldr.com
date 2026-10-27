---
title: "LFM3-1.7B beats Qwen3.5-9B on WebArena-Verified and runs in 1 GB"
slug: "liquid-lfm3-1-7b-agents-edge"
date: 2026-10-27T19:03:01+0000
summary: "Liquid AI released LFM3, six open-weight ~2B-class models (text, vision, audio, omni and two decision models) built on a new 1.7B backbone. By Liquid's own benchmarks, the 1.7B text model beats Qwen3.5-9B on web and business-app agent tasks (WebArena-Verified 38.4 vs 28.9) and on function calling, while larger models still win on agentic coding and knowledge. The text model fits in about 1 GB at 4-bit, has a 256K context window, and can be fine-tuned on a single GPU."
source: "https://www.liquid.ai/blog/lfm3"
source_title: "Introducing LFM3: Every Modality for Specialized Intelligence"
source_author: "Liquid AI"
source_site: "Liquid AI"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50262229"
---

Liquid AI released **LFM3**, a family of six open-weight models of about 2B parameters each, all branched from one new 1.7B base model trained on 22T tokens. The family covers text, vision, audio and omni input, plus two "decision" models specialized from the vision and omni models. Weights are on Hugging Face, with a hosted API and playground. All figures below are Liquid's own benchmarks.

**Architecture.** The 24-layer "tri-hybrid" design mixes 13 gated short convolutions (kernel size 7, up from 3 in LFM2), 5 sliding-window attention layers (256-token window) and 6 global attention layers. Other changes are head-wise gated attention, partial RoPE on global layers and sandwich norm. Liquid again used its hardware-in-the-loop architecture search, now across 14 device and backend combinations (phones, Raspberry Pi 5, laptops, cloud CPUs, GPUs and a Qualcomm NPU). With 8K tokens of context it runs about 25% faster than LFM2-1.2B.

**Results for the 1.7B text model** (256K context, about 1 GB at 4-bit):

- It scores 49.4 on Liquid's average, against 31.6 for LFM2.5-2.6B and 47.2 for Qwen3.5 9B.
- On agent tasks it beats Qwen3.5-9B on WebArena-Verified (38.4 vs 28.9), AutomationBench (41.3 vs 15.1) and LiveClaw (45.6 vs 35.8). It also leads on BFCL V4 function calling (71.4 vs 62.8).
- Larger models still win on agentic coding (SWE-bench Verified 43.4 vs 51.2 for Qwen3.5-9B), knowledge (GPQA Diamond 63.4 vs 80.4) and 128K long-context retrieval (RULER 65.6 vs 87.1).

The pitch is "specialized intelligence": small models you fine-tune locally on one GPU for a narrow job. The text model is post-trained for agent harnesses such as Hermes Agent, Pi and OpenClaw.
