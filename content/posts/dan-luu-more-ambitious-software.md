---
title: "Dan Luu: newer models make 1-year projects take a few hours of work"
slug: "dan-luu-more-ambitious-software"
date: 2026-10-25T00:02:49+0000
summary: "Dan Luu argues that current AI models have made much larger software projects feasible for one person, so builders should aim higher, even though most AI-built projects are low-quality slop. He built a custom regex engine, a ripgrep fork and a faster Rust build setup in a few hours of his own time, work he says would otherwise have taken a year or more."
source: "https://danluu.com/ambitious/"
source_title: "\"We should be more ambitious with the software we build\""
source_author: "Dan Luu"
source_site: "danluu.com"
hn_url: "https://news.ycombinator.com/item?id=50225695"
---

Dan Luu's bottom line is that model capabilities have advanced enough that builders can attempt much bigger projects than before. Most AI-generated rewrites are slop, but with reasonable testing and benchmarking the good ones can reach usable quality.

- **pgrust:** Michael Malis's Postgres rewrite in Rust looks like typical slop from the outside. Luu says Malis tests it thoroughly enough to have found real Postgres bugs, including a JSONPath format-cache bug, a pgcrypto bug and memory-safety bugs in the dictionary loader. Its main pitch is performance, and some of its optimizations could land in Postgres or ClickHouse even if pgrust itself fails.
- **Speeding up the agent loop:** Luu looked at what would bottleneck a 10x to 100x faster SOTA model: ripgrep, Rust compiles, and syscall and filesystem overhead. In a low single-digit number of hours of his time he built a tunable regex engine, a ripgrep fork using that engine's native-code compiler, and a setup that speeds up his Rust builds. These are "works for me" quality, not polished products.
- **Model quality matters:** a board-game AI took him about 20 hours with GPT-5.1/5.2, which often named techniques without implementing them correctly. A friend built a stronger one in less time with Fable 5.1 and Opus 5.5, which Luu says understand the field much better. He also reports GPT-6 Astra passing the zstd tests more than 99% of the time for about $1, so he moved his evals to the IMAP RFC and Pandoc.
- **Caveat:** Luu says "it just works" claims don't match his experience. Models can one-shot more, but he now works on harder problems, and the gap between what models can implement and what they can verify without handholding keeps growing.
- **Counterpoint:** Yossi Kreinin, quoted in an appendix, plans to argue that most people should be less ambitious, because many problems lack clear pass/fail criteria and people struggle to reason about large black boxes.

Luu notes he wrote the post quickly with little review and without rigorous benchmarks for his own projects.
