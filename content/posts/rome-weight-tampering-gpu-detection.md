---
title: "ROME attacks rewrite model weights in GPU memory with no file or network trace"
slug: "rome-weight-tampering-gpu-detection"
date: 2026-10-20T07:03:58+0000
summary: "A malicious Python library can apply a Rank-One Model Editing (ROME) attack by issuing ordinary CUDA kernels that overwrite a few weights in GPU memory, so a model gives corrupted answers while the checkpoint on disk is unchanged. Process-, file- and network-based security tools see a normal inference job. Security vendor Stealthium argues detection needs GPU-level visibility: tying each CUDA kernel launch to the Python call stack that issued it, and flagging in-place weight writes from non-loader code."
source: "https://stealthium.io/blog/hands-off-detecting-model-weight-tampering"
source_title: "Hands Off: Detecting Model Weight Tampering"
source_author: "Darek Stojaczyk, Rafael Tinoco"
source_site: "Stealthium"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50152976"
---

**Bottom line:** A poisoned Python dependency can change what a model says by editing a few of its weights directly in GPU memory. Nothing on disk changes, no process is spawned and no network traffic occurs, so EDR-style tooling sees nothing unusual. Stealthium (a vendor selling a detection product) shows a lab reproduction and how its runtime sensor caught it.

## The attack

- ROME (Rank-One Model Editing, Meng et al. 2022, also behind Mithril's PoisonGPT) makes a small targeted edit to one mid-layer weight matrix, changing the model's answer about a specific topic.
- In the demo, an imported library hooks PyTorch and applies the edit when the model enters eval mode. The same command on the same checkpoint then returns an answer with a different meaning.
- The edit is just extra CUDA kernel launches from the same `python3` process: run the model to trace it, solve a linear system for target weights, and write them into live weights.

## Why Python is the entry point

The post lists the ways code gets into GPU workloads: package imports that run code on import (it cites the 2026 `lightning` PyPI compromise and the LiteLLM `.pth` startup-file compromise), pickled model files that execute on load, and `exec`/`eval` or runtime-compiled kernels (an Open WebUI plugin hid its payload behind 64 layers of encoding).

## Detection approach

- Record CUDA kernel launches with the Python call stack behind them. The inference kernels look identical in clean and attacked runs, but the caller (`calibration.py::_calibrate` instead of the model's `reply`) and the extra kernels a clean run never launches are both anomalies.
- Record Python imports (with resolved file path and importer) and `exec`/`eval`/`compile` calls.
- Research in progress: hook the framework's in-place tensor-write signal to emit a "model weight mutation from a non-loader call path" event with the offending stack. Kernel launches alone can't say this, since their arguments are raw device pointers.

The authors note that instrumenting every Python monkey-patch would add overhead and noise, so they annotate GPU events with call stacks instead.
