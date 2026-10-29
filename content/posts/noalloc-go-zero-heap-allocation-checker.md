---
title: "noalloc: a Go checker that fails builds when a marked function heap-allocates"
slug: "noalloc-go-zero-heap-allocation-checker"
date: 2026-10-29T18:03:13+0000
summary: "The Consensus released noalloc, an experimental Go tool that checks the whole call tree under any function marked //noalloc:root and reports every line that allocates on the heap, so zero-allocation code stays that way as it changes. It ships fixed-capacity containers (Buf, Vec, Slice, Map, backed by Pool and Region) and a jsongen code generator for allocation-free JSON. The authors say the repo is written largely by AI and is used in their production web service and code search engine. It needs Go 1.27 or later."
source: "https://github.com/theconsensuslabs/noalloc"
source_title: "noalloc: Tooling and libraries to enforce zero heap allocations in Golang"
source_author: "The Consensus"
source_site: "github.com"
hn_url: "https://news.ycombinator.com/item?id=50278645"
---

**Bottom line:** noalloc turns "this function doesn't allocate" into something a tool can enforce. Mark a function with `//noalloc:root`, run `noalloc`, and it walks everything that function calls and lists each line that allocates, with the reason (map write, string concatenation, escape to heap). It exits 1 if it finds an allocation and 2 if it couldn't run. The README calls the project experimental and says it may change a lot.

## How it works

- **Directives:** `//noalloc:exempt <reason>` waives a line and everything it calls. `//noalloc:fixed` marks a type whose methods never grow its slices. `//noalloc:append` marks `append`-like helpers.
- **Allowed cases:** allocations on an error or panic path that ends the root's call, and first-use setup, are permitted. Calls outside your module pass if the callee's source (or compiled code, for assembly and the runtime) allocates nothing.
- **`bounded` package:** four fixed-capacity containers, `Buf`, `Vec`, `Slice` and `Map`. A write past capacity does nothing and marks the container full, so you check once at the end. Memory comes from your own slice or is borrowed from a `Pool` through a per-request `Region` with a fixed budget. Releasing the region returns the chunks.
- **JSON:** the `jsongen` command writes `WriteJSON` and `ReadJSON` methods for your types that allocate nothing, the first call included. It refuses types it can't handle without allocating, such as maps and interfaces.

## Numbers

The authors' benchmark, run on an Apple M4 with Go 1.27.0, decodes a 500-item feed (about 130 KB) in 1.16 CPU-seconds for 3,000 operations with zero allocations. `encoding/json` takes 2.94 s and makes 1,696 allocations. goccy/go-json is faster at 0.74 s with 4 allocations. Encoding the feed takes 0.87 s, against 1.52 s for `encoding/json`. The benchmark and the comparison are the authors' own.

## Caveats

The README lists several easy mistakes: using memory after `Region.Release` (a `Buf` or `Map` can't detect it and reads whatever the next borrower wrote), and keeping a slice from `Items` or `Bytes` across a write that moves to a larger chunk. Every place that can exhaust its budget also needs a plan, such as ending a page early or refusing oversized input.
