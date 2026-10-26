---
title: "Compiling Convex TypeScript functions to Wasm made them 2x faster, with 80% less memory"
slug: "convex-typescript-to-wasm-compiler"
date: 2026-10-26T20:05:00+0000
summary: "An independent developer built convex-wasm-compiler, which compiles Convex queries and mutations to WebAssembly with Static Hermes, plus a convex-backend change that runs them on Wasmtime. On their self-hosted app, queries and mutations are about 2x faster on average with 3x smaller tail latencies, and memory held for resident execution environments drops 80% versus cached V8 contexts."
source: "https://convex-in-prod.github.io/posts/convex-wasm-compiler/"
source_title: "Scaling self-hosted Convex: AOT compilation of TypeScript Convex functions into Wasm"
source_site: "Convex in Prod"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50249800"
---

**Bottom line:** The author compiles ordinary TypeScript Convex functions ahead of time to WebAssembly (using Static Hermes) and runs them with Wasmtime fibers on convex-backend's Tokio runtime. Results on their self-hosted app: queries and mutations about 2x faster on average, tails 3x smaller, and 80% less memory retained for resident execution environments than V8 contexts.

**The problem with V8:**
- Fast execution depends on reusing warmed V8 contexts. A cold function takes 100-150ms, versus 10-15ms warm.
- Contexts can't be shared between isolates, so the author keeps up to 400 cached at roughly 35MB each, about 14GB of memory.
- Deploys invalidate contexts, causing about a minute of re-warming that coincides with re-executed reactive queries, which can slow or shed executions.
- Tokio, V8 isolate and V8 background threads oversubscribe the CPU: roughly 75 active threads in a 28.5-CPU container, with CPU pressure peaking at 29% around deploys.

**Why Wasm:** Convex has said a Wasm runtime is planned, and a QuickJS-in-Wasm version briefly appeared in the repo in August, but it only interpreted the code. The conventional plan is for developers to write functions in Rust. This work compiles existing TypeScript instead. Embedding Wasmtime turned out to be only about 10% of a project that took roughly three months, five times longer than the author expected.

The post continues with the design, benchmarks and operational considerations of Wasm AOT compilation.
