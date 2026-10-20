---
title: "Rust compiler 2026: new trait solver, Polonius and a wave of speedups land"
slug: "rust-compiler-2026-payoffs"
date: 2026-10-20T16:02:35+0000
summary: "Rust compiler contributor Matyáš Racek says 2026 is when several long-running investments pay off at once: the next-generation trait solver (about 10 years in the making), the Polonius borrow checker, a parallel frontend close to stabilization, and a burst of performance work. LLVM 23 is also unambiguously faster, unlike several earlier upgrades. He also announces he is moving to work on the Rust project full time."
source: "https://yoyo-code.com/2026-is-exciting-year-for-rust-compiler-and-me/"
source_title: "2026 is an exciting year for the Rust compiler (and me)"
source_author: "Matyáš Racek"
source_site: "yoyo-code.com"
hn_url: "https://news.ycombinator.com/item?id=50166502"
---

**Bottom line:** the author argues 2026 is not special for the amount of Rust compiler work, but because many long-term efforts are finishing together.

## Big pieces

- **Next-gen trait solver:** a reimplementation of a large part of the type system, nearly 10 years in the works. It fixes soundness bugs and unblocks long-standing pain points, including async Rust and `impl Trait` limitations.
- **Polonius:** a new borrow checker that accepts more code and could eventually enable internal references. Its impact is smaller than the solver's.

## Performance

- **Parallel frontend** is close to stabilization. The author says almost nobody loves the design, but no better alternative exists, so the team ground through the hard problems.
- **LLVM 23** is clearly positive for rustc speed, after LLVM 22 was green on instruction counts but red on wall time.
- **Micro-optimizations** keep landing, helped by new contributors and better regression triage tied to merge queue work.
- **Move elimination:** a MIR pass that removes copies LLVM struggles to optimize, helping compile time and runtime.
- **Polymorphization:** a third experiment, this time Swift-style, passing generic layouts at runtime, with a flag to trade runtime cost for compile time.
- **Cargo:** `check` and `build` sharing a cache, `no-embed-metadata` by default (5-30% smaller target directories), a new build-dir layout enabling granular locking, and fixes to `no-link`/`link-only` for pipelining build scripts and proc macros.

## Architecture and process

Attribute handling was unified, the query system cleaned up, and const-generics groundwork advanced. The "project goals" framework is now working well, and new funding (a Maintainers in Residence program, a funding team) is arriving.

## The author

Racek, previously on perf triage, decided after the Rust All Hands to work on the compiler full time. He plans to take on the target-directory size reduction goal if funded, and notes there is no single employer to apply to.
