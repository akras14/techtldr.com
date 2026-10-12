---
title: "Intel's manual documented an FMA instruction no CPU runs, then dropped it"
slug: "vfmaddrnd231pd-intel-sdm"
date: 2026-10-12T23:02:22+0000
summary: "The 2024 Intel SDM documents VFMADDRND231PD, an FMA instruction with per-instruction rounding control, but it appears to have never existed in hardware: assemblers and compilers don't know it, and it raises #UD on a Haswell CPU. The 2026 SDM no longer lists it, and the author's guess is that Intel pulled it late and the docs were not fully cleaned up."
source: "https://purplesyringa.moe/blog/who-is-vfmaddrnd231pd/"
source_title: "Who is vfmaddrnd231pd??"
source_author: "purplesyringa"
source_site: "purplesyringa's blog"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50063138"
---

The 2024 Intel Software Development Manual documents `vfmaddrnd231pd`, a fused multiply-add that takes an immediate byte controlling rounding mode, DAZ, FTZ and exception suppression per instruction. That would be useful: x86 normally uses a global rounding mode that is slow to change, and since FMA can emulate add, subtract and multiply, per-instruction rounding would make some rounding-sensitive algorithms much faster without AVX-512. But the instruction seems not to exist.

- GNU assembler doesn't recognize it, and the `_mm_fmaddround_pd` / `_mm256_fmaddround_pd` intrinsics the SDM mentions are missing from Clang and from Intel's own compiler.
- Hand-assembling it from the SDM opcode entry (which uses an odd "DDS" VEX notation found nowhere else) and running it on a Haswell CPU raises #UD.
- The SDM lists only one variant, while other FMA instructions come in pd/ps/sd/ss and 132/213/231 forms.
- The 2026 edition of the SDM no longer contains it.

The entry has its own tables and pseudocode, so it doesn't look like a simple typo. The author's unconfirmed hypothesis is that Intel planned the `vfmaddrnd*` family, found a problem at the last moment, removed it from the silicon, and didn't fully correct the manual. They are asking anyone with an Intel contact to find out.
