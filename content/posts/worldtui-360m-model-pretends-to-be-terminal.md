---
title: "A 360M-parameter model fine-tuned to act as a terminal, running in the browser"
slug: "worldtui-360m-model-pretends-to-be-terminal"
date: 2026-10-22T06:03:06+0000
summary: "Simon Edwardsson fine-tuned a small language model (WorldTUI) to act as a terminal with no shell or filesystem behind it: it takes keypresses and predicts how the screen changes. It can create files, run vim, tmux and snake, and often remembers what you wrote earlier, but it is far from a reliable filesystem. The biggest gains came from training it to repair its own mistakes and from decoding screen rows in parallel; an int4 build (208 MB) runs locally via WebGPU."
source: "https://simedw.com/2026/10/21/world-model-terminal/"
source_title: "WorldTUI: I trained a 360M model to pretend to be a terminal"
source_author: "Simon Edwardsson"
source_site: "SimEdw's Blog"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50190363"
---

WorldTUI is a small \"world model\" experiment: instead of predicting video frames, the model predicts terminal screen updates from raw keypresses. It starts from Qwen2.5-Coder-0.5B, trained on about 336k synthetic sessions (roughly 2B tokens) recorded from real shells in Docker, and took about 75 GPU-hours on one RTX 4090.

**Design**
- The screen is a character grid. The model outputs which rows to rewrite plus the cursor position, using a vocabulary of just 333 tokens and grammar-constrained decoding.
- A `COPY` operation reuses rows from the previous screen, so a scroll costs about three tokens per row instead of roughly 96.

**What worked**
- **Self-repair (DAgger-style):** training on the model's own mistakes, with corrections only allowed when the needed text appeared earlier, raised PresetBench from 24/47 to 38/47 at int4.
- **Parallel rows:** one batch element per row, then speculative decoding where the same model verifies the parallel draft. Large-frame p95 latency fell from 6.7 s to 0.87 s on a 4090; a `man` page takes 0.9 s instead of 2.5 s in-browser on an M4 Mac.
- **Shrinking:** dropping unused embeddings took it from ~494M to ~360M parameters, and int4 quantization to 208 MB, run with custom WebGPU shaders.

**Results and limits:** on the author's tests it scored 80/106 on ShellBench and 9/13 file readbacks, versus 52 and 2 for the prompted Qwen2.5-Coder base. It still fails on novel command combinations (for example, `cat` not showing a line just appended with `>>`). Layer-pruned distillation kept plausible output but could silently destroy memory, so the author abandoned it.
