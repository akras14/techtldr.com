---
title: "Dioxus fullstack after a year: 90-minute CI builds, shared Rust types"
slug: "dioxus-fullstack-year-90-minute-ci"
date: 2026-10-28T02:02:45+0000
summary: "After a year running Dioxus 0.7 fullstack in production, Hauke Jung says the Rust backend is boring in the good way, but compile times hurt: a release image takes about 90 minutes on a 2-core GitHub runner (about 40 on a self-hosted one), and the WASM build alone takes roughly 22 minutes. For mostly dashboard-style apps, he plans to try Topcoat, a server-rendered Rust framework from the tokio-rs org, next."
source: "https://oxidt.com/blog/one-year-of-dioxus-fullstack-in-production-the-good-the-painful"
source_title: "One year of Dioxus fullstack in production: the good, the painful"
source_author: "Hauke Jung"
source_site: "OXIDT"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50265960"
---

**Bottom line:** A year of Dioxus fullstack in production left the author happy with the Rust code and unhappy with CI. One language and shared types across database, server functions and UI make refactoring safe, but build times are the cost.

## The good
- One language and toolchain from query to button; structs are defined once, and renaming a field makes `cargo check` list every broken spot, UI included. No OpenAPI spec or TypeScript interfaces to keep in sync.
- Desktop and mobile targets remain possible from the same components.
- Once it compiles it mostly just runs, so the bugs that reach users are logic bugs.

## The painful
- The dashboard compiles twice (server and wasm32). The WASM build alone takes about 22 minutes on 6 cores.
- A release image takes about 90 minutes on a 2-core GitHub-hosted runner, and about 40 on his own VPS runner.
- Clippy was OOM-killed on the 7 GB hosted runner; the fix was a 10 GB swapfile and `CARGO_BUILD_JOBS=1`.
- Caching with rust-cache helps, but GitHub only restores caches from the same ref or default branch and evicts after 7 days, so release builds kept starting cold. The mold linker barely helped. A self-hosted runner was the biggest win.
- "All Rust" has an asterisk: charts are Chart.js wrapped in about 340 lines of JavaScript, and Tailwind needs Bun in CI.

## What's next
For app-like needs (heavy client state, offline, mobile) he'd pick Dioxus again, but for forms-and-tables dashboards the WASM tax isn't worth it. His next project will use Topcoat, a server-rendered fullstack Rust framework from tokio-rs with no WASM bundle. It is early-stage and experimental, and he hasn't measured a build yet; a follow-up with numbers is promised.
