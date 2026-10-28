---
title: "Popcorn 1.0 runs Elixir in the browser on real OTP/BEAM compiled to a 730 KB Wasm"
slug: "popcorn-1-0-elixir-browser-otp-beam"
date: 2026-10-28T12:13:08+0000
summary: "Software Mansion's Popcorn 1.0 replaces AtomVM with the full OTP/BEAM compiled to WebAssembly, giving much broader OTP compatibility without patched standard libraries. The Wasm binary is 730 KB (Brotli) and boots in about 200 ms, and OTP runs the measured workloads 3.4x to 1,800x faster than AtomVM."
source: "https://swmansion.com/blog/popcorn-1-0-moving-from-atom-vm-to-otp-beam/"
source_title: "Popcorn 1.0: Moving from AtomVM to OTP/BEAM"
source_author: "Kuba Gonet"
source_site: "swmansion.com"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50263967"
---

**Bottom line:** Popcorn, a library for running Elixir in the browser, hit 1.0 by dropping AtomVM (a small embedded VM) for the real OTP/BEAM compiled to Wasm. Unmodified Elixir code now works, and the team no longer maintains a patched stdlib fork.

- **Why.** AtomVM implements only a subset of OTP, and a year of patching its standard library to run unmodified Elixir became too costly. A prototype of OTP-to-Wasm, built with LLM help over three days in January, proved it was feasible, though it first took about 10 seconds to boot.
- **Size and speed.** 730 KB Wasm binary (Brotli) plus about 2.5 MB of stdlib, which can be tree-shaken; a separate build includes crypto. Boot is about 200 ms (140 ms warm) vs AtomVM's roughly 80 ms. AtomVM starts 3.2x faster, but OTP runs the measured workloads 3.4x to 1,800x faster. The slow boot was mostly stdin reads sleeping 10 ms each across about 1,300 reads.
- **Architecture.** VM instances now run in Web Workers instead of iframes, allowing multiple instances and better isolation. The stdlib ships as a `.tar.br` of `.beam` files. `Popcorn.Fetch` is a Req adapter backed by the browser's `fetch()`, subject to CORS limits.
- **API.** `call`/`cast` now forward to `handle_call`/`handle_cast`, and a JS `send()` was added. Breaking changes from older versions are described as mechanical, with a migration guide in the docs.
- **Next.** PopDoc (interactive ExDoc plugin), Local LiveView improvements, tree-shaking, and faster support for new BEAM versions.
