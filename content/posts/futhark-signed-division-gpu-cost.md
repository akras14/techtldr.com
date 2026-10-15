---
title: "Fixing one signed-division overflow made Futhark's GPU loop 2.3x slower"
slug: "futhark-signed-division-gpu-cost"
date: 2026-10-15T13:03:02+0000
summary: "Futhark's generated C divided signed integers directly, so INT_MIN / -1 was undefined behaviour in C (and a CPU exception on x86). The fix adds a branch to every signed division. On the benchmark suite and on CPU there is no measurable cost, but a contrived division-bound CUDA program went from 5.5 ms to 12.6 ms."
source: "https://futhark-lang.org/blog/2026-10-14-defining-division.html"
source_title: "Making all divisions slower to fix a bug in 0.000000000000000000542% of cases"
source_author: "Troels Henriksen"
source_site: "The Futhark Programming Language"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50097797"
---

**Bottom line:** Futhark's compiler emitted plain signed C division, so the one input pair `most-negative-number / -1` was undefined behaviour in C and crashed with a CPU exception on x86. The fix is a branch on every signed division. It costs nothing measurable in real benchmarks, but about 2.3x in a contrived GPU microbenchmark.

- **Why it happens:** two's complement has no positive counterpart for the most negative value, so `x / -1` overflows. Signed overflow is undefined in C, and the compiler may assume it never occurs.
- **Existing approach:** Futhark defines integer overflow as wraparound, so it generates C with unsigned types, where overflow is well-defined. Division is the exception, because signed and unsigned results differ, so it used signed C division. The author says the bug sat there for ten years.
- **The fix:** `squot32` now checks `y == -1` and, if so, negates through unsigned arithmetic. The result is implementation-defined but correct on the target machines.
- **Cost:** no measurable change on the Futhark benchmark suite on GPU, and none on the sequential C backend (about 3.39 s before and after). On CUDA, a division-bound test program went from 5.5 ms to 12.6 ms, because the added branches stopped the compiler putting four `div.s32` instructions in one basic block. An AMD GPU on the HIP backend showed a similar but smaller effect.
- **Workaround:** use unsigned division if you know values are never negative.
- **Left over:** the author believes this was the last undefined behaviour in Futhark's generated C. Shifting by more bits than the integer's width is still implementation-defined.
