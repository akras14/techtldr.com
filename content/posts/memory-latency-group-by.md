---
title: "Radix partitioning cut a 99M-row GROUP BY from 3.6s to 2.7s on one core"
slug: "memory-latency-group-by"
date: 2026-10-31T20:06:00+0000
summary: "A 100% busy CPU is often mostly waiting on RAM. In a single-core GROUP BY over ClickBench's 99M-row hits table, a naive hash table spent 50% of CPU slots stalled on memory. Splitting rows into 4096 hash partitions first does more instructions but makes the hash tables cache-sized, cutting DRAM traffic from 4.5GB to 1.5GB and runtime from 3.6s to 2.7s."
source: "https://pivotlake.io/blog/its-the-memory-latency-stupid/"
source_title: "It's the memory latency, stupid"
source_author: "Maor Kern"
source_site: "Pivot blog"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50318720"
---
CPU utilization at 100% says little about useful work. A core can retire 400 to 800 instructions in the time one RAM access takes (about 50-100ns), so a workload that keeps missing cache looks busy while mostly waiting.

The author demonstrates this with `SELECT COUNT(*), UserID FROM hits GROUP BY UserID` on ClickBench's 99M-row dataset (about 17M unique users), pinned to one core of an AMD EPYC 9124. The naive approach, one growing hash table keyed by group, took 3.6s on a warm run. `perf stat` showed 50.1% of slots backend-bound on memory and only 16.1% retiring micro-ops, with about 4.5GB pulled from DRAM on demand loads.

The fix is radix partitioning. Phase one hashes each key and pushes it into one of 4096 partitions chosen by the top 12 bits of the hash. Phase two walks the partitions one at a time, aggregating each into a small hash table that fits in CPU cache. This does more total work (19.4B retired ops versus 11.4B), but memory stalls fell to 23.5%, DRAM loads fell to about 1.5GB, and the warm run took 2.7s.

The author's production engine, Pivot, reaches 1.85s on the same single core with about 954MB of DRAM traffic, and 172ms across 16 cores. Prefetching and memory-throughput-oriented algorithms are left for a follow-up on JOIN probing.
