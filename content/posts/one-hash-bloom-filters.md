---
title: "One-hash Bloom filters cut Bluesky deploys from 90 to 15 minutes"
slug: "one-hash-bloom-filters"
date: 2026-10-20T00:03:00+0000
summary: "Jim Calabro describes the Bloom filters Bluesky uses to skip database lookups for blocks, mutes and labels, and gloom, his Go implementation of a one-hash Bloom filter. Switching from a mutex-wrapped filter to a lock-free atomic one cut a full deploy from about 90 minutes to about 15; the one-hash variant has run in production for nine months."
source: "https://calabro.io/ohbf"
source_title: "One Hash Bloom Filters"
source_author: "Jim Calabro"
source_site: "calabro.io"
source_date: "2026-10-17"
hn_url: "https://news.ycombinator.com/item?id=50157865"
---

**Bottom line:** Bluesky uses Bloom filters to answer questions like "does user X block user Y" without hitting its Scylla database, saving millions of queries per second. At a roughly 1% positive rate, 99% of checks skip the database. Calabro's post walks through the filter variants and the one he now uses, a one-hash design implemented in his Go library gloom.

**What changed in production**
- The original filter (bits-and-blooms, double hashing) is not thread-safe, so every access went through a mutex. Filters are rebuilt at process start from full table scans while a firehose consumer keeps inserting, and pprof showed the mutex was the startup bottleneck.
- A colleague's fork, atomic-bloom, used atomic CPU instructions instead of locks. Full deploys dropped from about 90 minutes to about 15.
- Calabro notes that further hash savings would not have sped this up noticeably, because contention, not hashing, was the problem.

**How the one-hash filter works**
- Instead of k hash functions (or two, via Kirsch-Mitzenmacher double hashing), the bit array is split into k partitions whose sizes are pairwise coprime. One hash is taken modulo each partition size to get the k bit positions. The approach comes from a paper by Lu et al.
- gloom splits the filter into 64-byte blocks matching the CPU cache line, over-allocates and aligns them manually, so a lookup costs at most one cache miss. One 128-bit xxh3 hash picks the block (high bits) and the bits to test (low bits).
- It picks the block with a multiply rather than a modulo, using a trick from Daniel Lemire, and offers a sharded atomic variant that spreads writes across shards to reduce core contention.

gloom has run in Bluesky's backend for about nine months and is also used in jetstream, their sync service.
