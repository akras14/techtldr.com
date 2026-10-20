---
title: "tsrs, a Rust port of TypeScript, benchmarks up to 8x faster than TS 7.0"
slug: "tsrs-typescript-rust-port"
date: 2026-10-20T15:02:54+0000
summary: "Cloudflare, VoidZero and Owner.com's Max Schwenk are building tsrs, a faithful Rust port of the TypeScript compiler, incubated in the Oxc toolchain with the TypeScript team's cooperation. Early benchmarks, run with the TypeScript team on matched configs, claim up to 8x the speed and about 70% less memory than TypeScript 7.0."
source: "https://blog.cloudflare.com/typescript-rust-port/"
source_title: "Introducing tsrs: a Port of TypeScript to Rust"
source_author: "Boshen Chen, Dimitri Mitropoulos, Christoph Nakazawa"
source_site: "Cloudflare Blog"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50165953"
---

**Bottom line:** tsrs is a Rust port of the TypeScript compiler from Cloudflare, VoidZero and Max Schwenk of Owner.com. The authors say it is up to 8x faster and uses about 70% less memory than TypeScript 7.0 (the Go port) with the same number of type-check workers. It is experimental: `npm install tsrs`.

- **Not a fork.** The language stays under the TypeScript team's control; tsrs must follow upstream behavior strictly. Known differences remain, such as the order diagnostics print, and the authors aim to remove them before a stable release.
- **Benchmarks.** Configurations were matched with the TypeScript team. TypeScript 7.1 beta is already up to 2.5x faster than 7.0, partly thanks to optimizations upstreamed from tsrs. By default tsrs uses an adaptive config that spends more memory for more speed.
- **Why Rust over Go.** Mainly WebAssembly: Go's garbage collector must be compiled into Wasm, while Rust's memory model makes guest/host interaction simple and the footprint small. The goal is running full toolchains in sandboxes, browsers and embedded devices, including with agents.
- **Why now.** The authors say frontier AI models can do large ports almost independently in days, and describe a wave of "slopforks", faithful ports and re-architected versions.
- **Maintenance.** Agents will track upstream TypeScript daily and nightly, the way Cloudflare keeps Vinext in sync with Next.js. They are merging with Theo Browne's tsc-rs for LSP and content-mapper support, and plan to contribute performance improvements back to the Go codebase.
