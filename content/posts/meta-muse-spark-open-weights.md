---
title: "Meta releases Muse Spark Open: 816B-parameter open weights that run on two Mac Studios"
slug: "meta-muse-spark-open-weights"
date: 2026-10-15T18:03:00+0000
summary: "Meta released the weights of Muse Spark Open, an 816B-parameter mixture-of-experts model (42B active, 1M-token context) aimed at coding and agentic work. Meta says it runs on two Apple M5 Ultra Mac Studios via MLX, and calls it the most capable US open-weight model to date. The license is the custom Muse Model License with limited commercial terms."
source: "https://research.meta.ai/blog/muse-spark-open"
source_title: "Introducing Muse Spark Open, The Most Capable US Open-Weight Model to Date"
source_author: "Meta AI Research"
source_site: "Meta AI Research"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50106620"
---

Meta released the weights of Muse Spark Open, an 816B-parameter model with 42B active parameters and a 1M-token context window. It is natively multimodal and built for coding and agentic work. Meta calls it the most capable US open-weight model to date; that is Meta's own claim, and the post's benchmark details are not reproduced here.

**Running it locally.** Meta says two Apple M5 Ultra Mac Studios (up to 512 GB unified memory each, 1.2 TB/s bandwidth) can hold the whole model and run it via the MLX framework. Its demos ran fully on-device with Meta's Muse Code tool: shipping iOS changes to the open-source Mastodon client Ice Cubes (bug fixes, features, full build) and driving DaVinci Resolve to pick and crop clips from footage.

**Availability.** Hosted APIs are offered by Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI. Weights are on Hugging Face and the Amazon SageMaker, Google Cloud and Microsoft Foundry catalogs. It works with SGLang, vLLM, TensorRT-LLM, Transformers, MLX, llama.cpp and Unsloth. Meta also ships auto-tuned Helion GPU kernels for SGLang, claiming 1.03-3.95x kernel-level speedups and a 3% end-to-end gain on AMD MI350.

**License and safety.** The weights use the custom Muse Model License, which Meta says allows many developers to use the model free but has limited commercial terms. Meta says it added safety post-training for the open release and that the model met its scaling-framework thresholds; a full evaluation report is promised later.
