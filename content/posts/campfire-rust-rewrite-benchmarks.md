---
title: "Rust Campfire rewrite delivers 14% of events at 100 POSTs/s; the 256-slot channel is why"
slug: "campfire-rust-rewrite-benchmarks"
date: 2026-10-11T18:02:39+0000
summary: "A hands-on critique of DHH's AI-written Campfire rewrites in Rust and Elixir argues that vague prompts turn design decisions into coin flips, and that single-metric benchmarks mislead. At a constant 100 POSTs/s the Rust version delivered about 14% of events and Elixir about 60%, but Elixir timed out on about 23% of requests and hit 1.8GB of memory. The takeaway: you still have to think about failure modes and trade-offs."
source: "https://itsallaboutthebit.com/i-am-sorry-but-you-still-have-to-think/"
source_title: "I'm sorry, but you still have to think"
source_author: "Piotr Sarnacki"
source_site: "It's all about the bit"
source_date: "2026-10-11"
hn_url: "https://news.ycombinator.com/item?id=50043689"
---

**Bottom line:** The author reran benchmarks on DHH's AI-generated rewrites of Campfire (Rust, Elixir) and argues the results say little about the languages. Prompts that leave details open let the model pick answers at random, so the versions differ in backwards compatibility (the Rust one drops the CSRF token and Redis), and a single throughput metric hides the trade-offs.

- **Code quality:** the Rust version runs some database calls and sync locks directly inside async tasks, which blocks the cooperative scheduler. The Elixir version reportedly funnels SQL through one process.
- **Closed-loop vs constant-rate tests:** closed-loop benchmarks let the faster system receive more load. At a constant 100 POSTs/s, Rust delivered about 14% of events and Elixir about 60%. But Elixir timed out on about 23% of POSTs, and worst-case delivery latency was near 180s.
- **Root cause in Rust:** a bounded `tokio::sync::broadcast` channel with capacity 256. Raising it to 16384 lifted delivery to about 90%, but max latency went from 11s to over 130s. The author thinks failing fast and making clients reconnect is better.
- **Elixir isn't magic:** unbounded GenServer mailboxes pushed memory to 1.8GB in the same test, so you either drop messages or risk an OOM kill.

The author's conclusion: coding isn't "solved". You still need to set sensible bounds, weigh failure modes, and distrust one-number benchmarks.
