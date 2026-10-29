---
title: "A RISC-V CPU compiled into Qwen 3.5 4B's weights hits 100% on long-integer math"
slug: "qwenputer-riscv-cpu-in-qwen-weights"
date: 2026-10-29T02:02:24+0000
summary: "Divyansh Agrawal compiled a 32-bit RISC-V CPU directly into the weights of Qwen 3.5 4B, using the model's Gated DeltaNet layers as exact RAM. No training and no external tool or sandbox is involved. On NUPA long-integer arithmetic it goes from 48.6% to 100%, matching a Python tool."
source: "https://divagr.com/blog/qwenputer"
source_title: "Qwenputer: a fully functioning, deterministic RISC-V CPU inside Qwen3.5"
source_author: "Divyansh Agrawal"
source_site: "divagr.com"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50282838"
---

**Bottom line:** Qwenputer is an experimental build of Qwen 3.5 4B with a RISC-V CPU written straight into its weights. The CPU runs inside the model's own forward passes, so arithmetic, C and MicroPython programs execute without a plugin, tool call or sandbox.

**How it works**
- Three of every four Qwen 3.5 attention layers are Gated DeltaNet (a linear RNN). Setting the gate to 1, the write strength to 1 and using one-hot keys makes each write overwrite exactly one state row and each read return an exact value. That turns the layers into addressable RAM whose size doesn't grow with conversation length, unlike a KV cache.
- No training is involved. A compiler sets every weight directly from a circuit description of the CPU, adding 11 layers in front of Qwen's 32. Qwen's original weights are untouched.
- Each CPU instruction becomes a few generated tokens (address, value, step), so the CPU runs while the model decodes. Qwen invokes it by reusing the tool-call format it already knows.

**Results**
- The core implements full RV32IM, passes the official riscv-tests, matches qemu, and ran 18 million instructions free-running without error. About 230,000 output positions replayed against a reference machine showed zero mismatches.
- On NUPA long-integer arithmetic, accuracy goes from 48.6% to 100%, the same as giving Qwen a Python tool. On DeepMind arithmetic sets it scores 97.5-100%, versus 55-90% for base Qwen.
- Normal text output stays within noise of base Qwen (KL 6e-4 against a 5e-4 noise floor).
- An experimental in-model filesystem (up to 512 files, about 53,000 tokens) persists across conversations. It answered changed facts correctly 90% of the time, against 30% when old chats were pasted back in.

**Limits:** the checkpoint is 18.8 GB, about double base Qwen. It needs a small driver (RvChat) rather than a plain `generate()` call, and it's slow: each CPU instruction costs a few decoded tokens, about 3,200 CPU tokens/s on an L4. It runs on a single 24 GB GPU. The author also wrote follow-ups on hybrid models hiding reasoning in RNN state and on compiled in-weight monitors.
