---
title: "Muse Spark Open hits 36+ tokens/s on four Mac Studios with MLX over Thunderbolt RDMA"
slug: "apple-mlx-rdma-muse-spark-mac-studio"
date: 2026-10-16T16:04:00+0000
summary: "Apple's ML research team shows MLX running the 816B-parameter Muse Spark Open (4-bit NVFP4, about 493GB) across four M5 Ultra Mac Studios at over 36 tokens/s, using the RDMA-over-Thunderbolt 5 driver in macOS 26.2 and MLX's new jaccl backend. On a single machine, the M5 Ultra's GPU Neural Accelerator cuts time to first token by up to 4.6x versus the M3 Ultra, and decode is up to 1.6x faster thanks to 1.2 TB/s of memory bandwidth. Tensor parallelism needs the low-latency link and scales better for dense models than for the MoE."
source: "https://machinelearning.apple.com/research/mlx-distributed-inference"
source_title: "Local and Distributed Large LLM Inference with MLX and RDMA on Mac Studio"
source_author: "Apple Machine Learning Research"
source_site: "Apple Machine Learning Research"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50118320"
---

**Bottom line:** Apple says the newest MLX release can run Meta's Muse Spark Open (816B parameters, 42B active) at over 36 tokens/s on a cluster of four M5 Ultra Mac Studios, by connecting them with RDMA over Thunderbolt 5. It is Apple's own benchmark; see our [Muse Spark Open post](/posts/meta-muse-spark-open-weights/) for the model itself.

## Single machine

- Qwen 3.8 (27B) and Mistral Medium 3.5 (128B) fit on one 512GB Mac Studio, using up to 68GB and 158GB at a 65,536-token context.
- Versus an M3 Ultra, the M5 Ultra's GPU Neural Accelerator gives up to 4.6x faster time to first token (compute bound). Decode, which is memory-bandwidth bound, is up to 1.6x faster (1.2 TB/s vs 819 GB/s).
- Peak power is about 300W on M5 Ultra and 200W on M3 Ultra.

## Multiple machines

- Muse Spark Open needs over 1.6TB in BF16 and about 493GB in NVFP4 before working memory, so it can't fit on one Mac Studio.
- Macs are linked with two Thunderbolt cables each. macOS 26.2 added a low-latency RDMA driver, and MLX (v0.30.1+) adds `jaccl` collectives on top; `mlx.launch --backend jaccl` starts jobs across hosts.
- **Pipeline parallelism** (split by layers) leaves decode speed about unchanged, but speeds up prefill on long contexts by up to about 1.8x at 65,536 tokens, because chunks overlap across machines.
- **Tensor parallelism** (split each layer) is what improves decode speed, but only works well with very low-latency links. Dense models scale better than Muse Spark Open, where the experts' output normalization adds prefill communication.
- The reported four-Mac result is at an 8,192-token context.
