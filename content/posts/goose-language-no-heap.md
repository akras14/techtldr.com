---
title: "Goose: a memory-safe systems language with no heap, GC or lifetime annotations"
slug: "goose-language-no-heap"
date: 2026-10-24T21:02:25+0000
summary: "Goose is a new memory-safe systems language by Wouter van Oortmerssen (aardappel) that has no heap allocator, garbage collector or lifetime annotations. Its own 16-benchmark suite shows about 3.5x the speed of idiomatic C++, 1.24x hand-optimized C++ and 1.13-1.19x the best safe Rust, with 1.25-1.95x less peak memory (the author's figures, not independently verified)."
source: "https://aardappel.github.io/goose/"
source_title: "The Goose Programming Language"
source_site: "aardappel.github.io"
hn_url: "https://news.ycombinator.com/item?id=50224300"
---

**Bottom line:** Goose is a systems language with C/Rust-like syntax that drops the heap entirely. Every dynamic value lives inline on compiler-managed data stacks, and freeing happens at scope exit. The project claims memory safety with no lifetime annotations, no `unsafe`, and no aliasing rules. Its benchmark suite reports roughly 3.5x idiomatic C++ speed, 1.24-1.26x hand-optimized C++ and 1.13-1.19x the best safe Rust, using 1.25-1.95x less peak memory. These are the author's own results.

**How it works**

- **Data stacks:** growth is a pointer bump and the "free" is one store, so a reference into a growing array stays valid. Nothing moves.
- **Inferred references:** the compiler infers what each reference is rooted in and rejects ones that could outlive their owner.
- **Inline data:** nested strings, arrays and variable-size records are one contiguous block. The example order record is 29 bytes with no allocations, versus 160 bytes and one allocation in C++.
- **Compact types:** enums take the size of their variant, and relative links can be 1, 2 or 4 bytes. Data structures are position independent, so saving is a write and loading is a read plus verification.
- **Threads share nothing:** workers are compiled as separate programs and pass flat values over typed queues, so data races and locks don't exist.
- **Compiles to C:** it outputs one C file, calls C directly, and bundles a TinyCC backend to run programs in-process.

**What it costs:** you have to think about where data lives. Recursive functions can't keep growable data across a recursive call. Arrays of variable-size elements can be iterated but not indexed. There are no escaping closures, function pointers or dynamic dispatch beyond enum tags, and compilation is whole-program only.
