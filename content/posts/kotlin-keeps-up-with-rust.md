---
title: "Kotlin Keeps Up With Rust"
slug: "kotlin-keeps-up-with-rust"
date: 2026-10-09T22:09:25+0000
summary: "The author ported the Rust version of 37signals' Campfire to Kotlin in about a day with Claude Code, byte-for-byte identical in output. On Hetzner x86 servers the JVM build matched or beat Rust on the room page at most connection counts, though Rust still wins on memory, startup and tail latency after warmup. The author argues DHH's benchmark table mostly reflects how much optimization attention each port received."
source: "https://www.wecodefire.com/p/kotlin-keeps-up-with-rust"
source_title: "Kotlin Keeps Up With Rust"
source_author: "Mystic"
source_site: "wecodefire.com"
source_date: "2026-10-09"
---
The author's claim is that DHH's Campfire benchmark table, which ranks Rails, Django, Laravel, Express, Elixir, Go, Rust and C by requests per second, measures effort more than language. Rails went from 230 to 4,101 requests/s in a day with the same language and hardware, purely from architectural changes carried across ports.

**The experiment:** a Kotlin port (about 22,000 lines, on the JVM or as a GraalVM native image) of the Rust port, serving the room page only. It uses Netty, Java's FFM API for SQLite and a Kotlin port of the html5ever parser. The hard rule was byte-identical responses, headers included, verified against the Rust output (a 658-case corpus plus 180,000 fuzzed cases, and every response under load). It took roughly 24 hours with Claude Code and about 4.65 euros of cloud spend.

**Results** (Hetzner AMD EPYC, closed-loop load, medians, Rust patched to use a larger connection queue):
- Without a response cache, the JVM beat Rust on one core by 2% to 22% and on two cores by 25% to 48%. The native image was roughly between 84% and 125% of Rust.
- With the upstream response cache ported, everything roughly doubled and the ordering held: JVM about 3% to 36% ahead on one core and mostly 13% to 27% ahead on two, using less CPU per request. At 1,024+ connections network packet loss on Hetzner's virtual network distorted results.

**Where Rust still wins:** memory (15 to 17 MB idle versus 117 to 170 MB for the JVM), startup (about 175 ms versus 1.5 s), and p99 latency after warmup (2 to 3 ms versus 31 to 80 ms at a fixed 4,000 requests/s). The author also notes the Rust port is not a strictly even code comparison and that the Kotlin version reads more concisely to them.
