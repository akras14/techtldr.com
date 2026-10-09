---
title: "Reducing Undefined Behavior in C"
slug: "reducing-undefined-behavior-in-c"
date: 2026-10-09T22:06:33+0000
summary: "At Kernel Recipes, Martin Uecker said C's undefined behavior is steadily shrinking: C23 banned compiler 'time travel' and C2y has removed 45 of roughly 100 undefined-behavior cases. Better warnings, analyzers and sanitizers help too, but full memory safety will need runtime checking or formal verification."
source: "https://lwn.net/Articles/1095811/"
source_title: "Reducing undefined behavior in the C language"
source_author: "Jonathan Corbet"
source_site: "LWN.net"
source_date: "2026-09-28"
---
Uecker, a biomedical engineering professor and C standards participant, argues C is still worth using in 2026: it is portable, stable, fast to compile, and its output is predictable. Its weak spot is undefined behavior (UB), a legacy of the C89 abstract-machine model, which let compilers assume UB never happens.

- **The problem.** Compilers may treat any program containing UB as having no meaning, and developers disagree about what to expect, for example when reading struct padding. Some compilers even hoisted an undefined division above an earlier observable action ("time travel"). C23 now forbids that.
- **Progress.** The committee has study groups on the memory object model, memory safety and UB. About 100 UB cases exist in the standard, and the C2y draft has removed 45. C23 also dropped K&R definitions, trigraphs and non-two's-complement integers, and added checked integer operations. C2y adds features like case ranges and `_Countof()`.
- **Tools.** Compiler warnings, built-in static analysis, sanitizers (which can also harden in trapping mode), LLM-based tools and formal verification are all improving.
- **Memory safety.** Type safety is fixable with annotations and checks. Bounds checking is partly solved (for example `counted_by`). Temporal safety, such as use-after-free, is hardest, and Rust has the edge; CHERI and Fil-C help.

Uecker's conclusion: full safety will need expensive runtime checks or formal verification of a restricted language. C won't get there soon, but it is a living language and becoming safer over time.
