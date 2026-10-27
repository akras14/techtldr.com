---
title: "Four months back in Zig: explicit IO and allocators win, instability and docs lose"
slug: "four-months-zig-vs-rust"
date: 2026-10-27T22:02:32+0000
summary: "A student returning to Zig after years away, and coming from Rust, likes its explicit allocator and IO parameters, comptime, build system, small dependency trees and incremental compilation. The pain points are standard library churn before 1.0, thin documentation, missing lifetime checks, a fuzzer that doesn't work yet, and a bootstrap process that still relies on a checked-in WASM blob."
source: "https://gracefulliberty.com/articles/four-months-of-zig/"
source_title: "Four Months of Zig"
source_author: "Anna Liberty"
source_site: "gracefulliberty.com"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50265128"
---

After about four months of writing Zig again (a smallnet client library, a TUI typing game, an n-body simulator), the author reports that Zig's explicitness is the main draw, and that stability and documentation are the main costs. Rust was their previous default, so most comparisons are against it.

**What they like**
- **Explicit allocators and IO.** Every allocating function takes an allocator, and every function that touches the OS takes an IO parameter. That makes it obvious which code is pure, makes tests easy (swap in a test IO that fails writes or feeds input), and makes porting to a new OS mostly a matter of porting IO.
- **Comptime, build system, dependencies.** Comptime covers metaprogramming and precomputation in one feature. The build system integrates other tools more easily than Cargo, and the dependency culture is "avoid": their programs have one or two direct dependencies and none indirect, because reflection, RNG and JSON are in the standard library.
- **Error handling.** `try` plus `!` is the laziest option and is still correct, whereas quick Rust prototypes tend to leave panics behind.
- **Incremental compilation.** `zig build --watch -fincremental` gives fast rebuild loops, though it currently only works on x86_64 Linux and macOS. The author also praises `anyzig` for switching between Zig versions, with the caveat that it needs a `build.zig.zon`.

**What they want fixed**
- **Stability.** Standard library breakage before 1.0 is the main reason they still use Rust for some projects.
- **Lifetimes.** Returning a local buffer isn't caught even in debug builds, and the compiler accepts block-scoped stack buffers that may break in a future version.
- **Documentation.** Sparse doc comments, few examples, and knowledge scattered across the web. The author couldn't get the standard library's TLS working in their project.
- **Compile-time allocators.** JSON can't be parsed at compile time because no allocator works there.
- **Fuzzing.** The built-in fuzzer, shown in the `zig init` template, isn't finished.
- **Bootstrapping.** The compiler now bootstraps from a checked-in WASM blob instead of C++. That is better than needing a previous compiler, but the blob can't be audited from source, and distributions like Debian can't package Zig under their rules. The author chose another language for one project for that reason.

This is a personal experience report, not a benchmark, and the comparisons to Rust reflect the author's preferences.
