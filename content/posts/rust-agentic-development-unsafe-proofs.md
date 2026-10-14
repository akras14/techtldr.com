---
title: "Niko Matsakis: Rust's agent-era bet is proving unsafe code sound"
slug: "rust-agentic-development-unsafe-proofs"
date: 2026-10-14T12:03:17+0000
summary: "Rust language designer Niko Matsakis argues that humans still have a role in programming and that Rust suits agent-written code: fast by default, tunable with a profiler, and guarded by a strong type system. He proposes a north star of \"no more unsafe code\", meaning every unsafe block is proven free of undefined behavior with tools like Verus, Creusot, Aeneas or Kani, plus a more extensible compiler to host those tools."
source: "https://smallcultfollowing.com/babysteps/blog/2026/10/14/agentic-dev-and-rust/"
source_title: "What agents mean for Rust (and vice versa)"
source_author: "Niko Matsakis"
source_site: "baby steps"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50084416"
---

Matsakis, a longtime Rust language designer, writes that LLMs haven't made programming or language design irrelevant, and that Rust is a good fit for agentic development. He cites David Tolnay's observation that Meta engineers wrote twice as much first-party Rust this year as in the previous nine years combined. The post is a written companion to his REBASE talk at SPLASH.

He frames agent-driven software as three pillars: design, construction and optimization.

- **Optimization:** Rust is fast by default and exposes knobs (allocation, layout, inline assembly). Agents are good at using them, for example by reading profiler output (he names dial9) to find async pitfalls. He says this means Rust can tolerate slightly noisier code, such as explicit `clone()` calls.
- **Reliability:** "If it compiles, it works" keeps agents on track where tests alone don't. But type systems only rule out some wrong things. Proof tools (Verus, Creusot, Aeneas, Kani, or theorem provers like Lean) can show richer properties, and Verus leans on the borrow checker's uniqueness guarantees to make proofs easier.
- **Design:** he calls this an open problem, currently mostly "markdown-driven development", and suggests separating interface from implementation for review and dedicated modeling languages.

His proposed north star is "no more unsafe code": still using the `unsafe` keyword, but being able to prove that all of it is sound, motivated by agents' ability to find vulnerabilities. To get there he wants a more expressive type system (for example "this method only reads one field", or "no logging of PII") and an extensible compiler so proof tools stop depending on janky macros and pinned compiler versions.
