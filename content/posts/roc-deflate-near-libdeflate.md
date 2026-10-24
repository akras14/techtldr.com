---
title: "Pure Roc DEFLATE is 2.6–8.9% slower than C libdeflate at median"
slug: "roc-deflate-near-libdeflate"
date: 2026-10-24T06:02:39+0000
summary: "Richard Feldman optimized roc-deflate, a memory-safe, pure-function DEFLATE package for the young Roc language, until it came within single digits of libdeflate, the SIMD-tuned C champion. Across 144 benchmarks on the Silesia corpus, the median slowdown was 8.9% on a Ryzen 3900X and 2.6% on an M1 MacBook Pro. Worst cases were 18% slower and best cases slightly over 18% faster. He argues the remaining gap is a fair price for guaranteed memory safety, no side effects, deterministic behavior across targets, and no build scripts or arbitrary FFI. He cautions that this is one result and may not generalize."
source: "https://rtfeldman.com/fp-vs-c"
source_title: "How close can pure FP get to C?"
source_author: "Richard Feldman"
source_site: "rtfeldman.com"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50218195"
---

**Bottom line:** A pure-functional, memory-safe Roc package for DEFLATE compression gets within single digits of libdeflate, the heavily tuned SIMD C library, at the median: 8.9% slower on a Ryzen 3900X and 2.6% slower on an M1 MacBook Pro, across 144 benchmarks on the Silesia corpus.

## What was done

- The follow-up to an article showing a Lean 4 zip library beating Rust's miniz_oxide but losing badly to libdeflate.
- Feldman spent a stretch optimizing roc-deflate (by Niclas Ahden) on the non-streaming APIs, checking that outputs matched libdeflate's.
- Roc is still pre-0.1.0 and its package ecosystem has had little optimization work.

## Results

- Median slowdown: 8.9% (Ryzen 3900X), 2.6% (M1).
- Worst-case input: 18% slower. Best-case input: just over 18% faster. He attributes the wins to implementation differences, not a Roc advantage.
- He concedes C's performance ceiling is higher: Roc has no unsafe opt-out from bounds checks or reference counting.

## Why accept the gap

- Memory safety with no opt-out, so no unchecked-memory bugs like the Brotli RCE he cites.
- Types distinguish pure from effectful functions, and the package is 100% pure.
- No arbitrary FFI and no package build scripts, which limits supply-chain damage.
- Deterministic results across compile targets.

## Roc-specific optimizations

- Keep values unique so opportunistic in-place mutation applies.
- Add a redundant-looking up-front length check so the optimizer can drop bounds checks in loops.
- Build lookup tables with compile-time evaluation and match exhaustively on them.
- Use `plus_wrap` instead of `plus` where overflow can't happen and the compiler can't prove it.

He notes this is a single data point and that x64 likely has more headroom.
