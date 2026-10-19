---
title: "Arch Linux ARM on an RTX Spark laptop matches Windows in 7-Zip and sees 116 GiB to CUDA"
slug: "arch-linux-arm-rtx-spark-laptop"
date: 2026-10-19T22:05:00+0000
summary: "Arch Linux staff member Josephine Pfeiffer got Arch Linux ARM running on the ASUS ProArt P16 (128 GB, 20-core N1X, RTX Spark), a laptop NVIDIA officially supports only on Windows. Her 7-Zip runs land within about 1% of published Windows results single-threaded and 3-5% ahead on all 20 threads, and Linux exposes one 116.0 GiB pool to both the OS and CUDA instead of Windows' split reservations."
source: "https://josie.lol/blog/linux-rtx-spark"
source_title: "putting linux on my rtx spark laptop because i don't have a microsoft account"
source_author: "Josephine Pfeiffer"
source_site: "josie.lol"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50158100"
---

**Bottom line:** Josephine Pfeiffer, an Arch Linux package maintainer, built a bootable Arch Linux ARM setup for the ASUS ProArt P16 (128 GB RAM, 20-core N1X, RTX Spark). The hardware is officially supported only on Windows. In her benchmarks Linux keeps up with Windows and handles unified memory more flexibly.

## What it took

- Started from NVIDIA's Ubuntu kernel work, then fixed the ACPI override allocation and framebuffer mapping to get a console, and derived ACPI overrides from the laptop's own tables.
- Needed a patched `hid-asus` for the keyboard and a dedicated audio machine driver; the NVIDIA driver and ASUS speaker firmware come from official sources.
- Packaged as a bootable USB plus an extended Archinstall 4.5. Build inputs are hash-pinned and the hardware packages are signed with her maintainer key. Scripts and patches are in her repo.

## Results

- **7-Zip (native ARM64, 32 MiB dictionary):** about 1% behind published Windows numbers on one thread, about 5% ahead on compression and 3% ahead on decompression with 20 threads. The Windows comparison is against another laptop with the same chip, with unspecified 7-Zip settings.
- **Memory:** Linux reports 116.0 GiB as both system RAM and CUDA total. On Windows the RAM is split into dedicated GPU, shared and CPU-only regions; one reviewer's 128 GB unit reached a 109.7 GiB CUDA total with a 91 GiB reservation.
- **Local LLM:** NVIDIA's Qwen3.8-Flash-Next FP4 checkpoint on vLLM used nearly 90 GiB for model and KV cache, with a ~16 minute load. Generation ran at about 31 tokens/s, versus roughly 29-31 tokens/s on Windows with llama.cpp. She notes the quants, runtimes and context sizes differ, so this doesn't isolate the OS. All 171 measured responses passed her checks, and latency drifted under 1.6% over a ten-minute run.
