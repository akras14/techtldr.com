---
title: "New RubyGems names doubled year over year as AI lowers the cost of building libraries"
slug: "ruby-renaissance-gem-names-doubled"
date: 2026-10-12T17:03:09+0000
summary: "Rubyist Julik Tarkhanov answers DHH's RailsWorld keynote, which he read as a shrug at the Ruby ecosystem, with an argument that LLM coding tools are making Ruby better. Problems that long went unsolved, such as PDF layout, Parquet, gRPC and native GUI bindings, are being fixed in days rather than months. By his count of the RubyGems dataset, organic new gem names rose from about 4,600 to about 8,900 year over year, a step up in early 2026 rather than a slope."
source: "https://blog.julik.nl/2026/10/the-weimar-of-ruby"
source_title: "Living in the Weimar era of Ruby"
source_author: "Julik Tarkhanov"
source_site: "blog.julik.nl"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50054474"
---

**Bottom line:** the author argues the Ruby ecosystem is in a creative boom, not decline, because AI-assisted coding makes it cheap to build the libraries Ruby has always lacked. He frames this as a response to DHH's RailsWorld keynote, which he heard as "we vibe-rewrote HEY, we no longer care about Rails."

- **Long-standing gaps are closing.** Two PDF generators appeared in a few months: `sghtmltopdf` (a Rust layout engine built on Servo and Typst crates, a replacement for wkhtmltopdf) and `stationery` (a Phlex-style Ruby DSL). He moved his invoicing to them in about an hour and gets double-digit-millisecond generation. Qt bindings, Scarpe (a revived Shoes), RMagick releases and an Async-friendly gRPC are also in progress.
- **His own example:** he wrote a full Parquet library, `herringbone`, in a few days. The library streams, uses native compression and supports Parquet encryption. He says his earlier ZIP library took about three months, and the Parquet spec is roughly five times larger. He says you can't one-shot such a library: you still need to know what you want and iterate on the API.
- **The numbers:** using the RubyGems dataset hosted by ClickHouse, he counted organic new gem names (excluding unindexed names and large prefix families from a few accounts). About 4,600 appeared in the year to October 2025 versus about 8,900 in the year since, 1.9x. Monthly counts went from roughly 400-500 through 2025 to 800-1,600 from March 2026. He can't say whether these are new people or the same people more productive, and he leaves quality to the reader.
- **Against Rust rewrites:** like José Valim in a quoted post, he asks why people rewrite apps in Rust instead of making Ruby faster or better at concurrency, and points to performance work such as Sam Ruby's Roundhouse cross-compiler and a Campfire rewrite in vanilla Ruby reported 15-27x faster.

His conclusion: use the tools to fix the old annoyances today, without waiting for permission.
