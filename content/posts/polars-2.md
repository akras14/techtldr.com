---
title: "Polars 2.0 streams by default and leads DuckDB in its own TPC-H tests"
slug: "polars-2"
date: 2026-10-10T21:31:50+0000
summary: "Polars 2.0 makes the streaming engine the default for lazy queries, turns on spill-to-disk, treats SQL as a first-class interface, and adds a Map dtype. In the team's own TPC-H- and TPC-DS-derived benchmarks, Polars beat DuckDB and DataFusion on all but one run. The main breaking change: some operations no longer guarantee row order unless you ask."
source: "https://pola.rs/posts/release-polars-2/"
source_title: "Release of Polars 2.0"
source_author: "Ritchie Vink"
source_site: "Polars"
source_date: "2026-10-06"
hn_url: "https://news.ycombinator.com/item?id=49977177"
---

Polars 2.0 makes the streaming engine the default for lazy queries, turns on spill-to-disk, treats SQL as a first-class interface, and adds a Map dtype. In the team's own TPC-H- and TPC-DS-derived benchmarks, Polars beat DuckDB and DataFusion on all but one run. The main breaking change: some operations no longer guarantee row order unless you ask.

## Streaming and out-of-core by default

- `collect()` on a LazyFrame now uses the **streaming engine**, which the team says brings large memory and speed gains on most queries.
- That's why this is a major version: streaming doesn't guarantee row order for some operations (`join`, `group_by`, `unpivot`, etc.). Set `maintain_order=True` if you need it.
- **Spill-to-disk** is on by default, starting at about 80% of RAM with a 64 GB disk budget. It covers sorts, window functions and many expressions now; joins and group-bys are next.

## SQL and performance

SQL coverage has grown a lot, backed by optimizer work: join reordering, better common-subplan elimination, and dynamic predicates/bloom filters.

Benchmark setup: Polars SQL vs. DuckDB 1.5.6, a DuckDB 2.0 alpha and DataFusion 54.0.0, on a 16-vCPU c7a.4xlarge and a 192-vCPU c7a.metal, best of five hot runs per query. Results:

- Default Polars was fastest on all but one benchmark.
- At 192 threads, Polars has a constant overhead that hurts small-data queries. Limited to 32 cores it was competitive or winning everywhere. The team says it has found the cause.
- Going from 16 to 192 vCPUs at SF100, Polars got 3.8x faster on TPC-H and 2.2x on TPC-DS, vs. 3.2x and 1.9x for DuckDB 1.5.6.
- DataFusion timed out or ran out of memory on a few queries, which were excluded for all engines.

The benchmark repo is public. As always with vendor-run benchmarks, treat them as a claim to replicate.

## Map dtype and stricter behavior

- **Map** now maps directly to Arrow's MapType (previously read as a list of key/value structs), with methods like `map.get`, `contains_key`, `keys` and `values`.
- Polars is stricter about dtypes and implicit conversions so errors show up early. `collect_schema()` catches schema mismatches without materializing data, which the team pitches as faster feedback for both humans and AI agents.

Next up: out-of-core joins and group-bys, better scaling at high core counts, Polars Cloud, and GeoPolars. A migration guide is available.
