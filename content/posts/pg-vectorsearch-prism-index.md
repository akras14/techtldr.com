---
title: "pg_vectorsearch's PRISM index builds 100M vectors in 1h40m on 128 GB"
slug: "pg-vectorsearch-prism-index"
date: 2026-10-14T13:03:14+0000
summary: "Timescale (Tiger Data) released pg_vectorsearch, an alpha open-source Postgres extension whose partition-based PRISM index aims to beat pgvector's HNSW with far less memory. On 100 million Cohere embeddings, PRISM built in 1h40m on a 128 GB machine where HNSW could not build at all, and hit 6 ms p50 latency at 95% recall. These are the vendor's own benchmarks, and the extension is not production-ready."
source: "https://www.tigerdata.com/blog/announcing-pg_vectorsearch-scalable-open-source-vector-search-postgresql"
source_title: "Announcing pg_vectorsearch: Scalable Open-Source Vector Search in PostgreSQL"
source_author: "Erik Nordström, Matvey Arye, Noah Hein"
source_site: "Tiger Data"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50086165"
---
**Bottom line:** pg_vectorsearch is a new open-source Postgres extension (alpha, "meant for testing, not production") that swaps HNSW's in-memory graph for PRISM, an IVF-style partitioned index with RaBitQ quantization. Tiger Data says it needs much less RAM and builds far faster than pgvector, while staying inside Postgres.

## Reported numbers (vendor benchmarks, recall@10)

- **100M vectors, 128 GB machine:** PRISM built the index in 1h40m and served 95% recall at 6 ms p50 (21 ms at 99% recall). HNSW could not build on that machine.
- **100M vectors, 512 GB machine (4x the cost):** HNSW finished after 42 hours and served 95% recall at 8 ms; the authors could not get it to 99% recall at a usable latency. PRISM built in 49 minutes and hit 99% recall in 19 ms.
- **Index size:** 25 GB for PRISM vs 180 GB for HNSW at 100M vectors.
- **1M vectors, 99% recall:** 15 ms on a 2 GB machine for PRISM vs 392 ms for HNSW on a machine with twice the memory.

HNSW runs used pgvector's 16-bit `halfvec`, which the authors say gave it the best tradeoff.

## How it works

PRISM groups similar vectors into clusters found via a hierarchical centroid tree kept warm in the Postgres buffer cache. Compressed vectors sit in posting lists laid out for SIMD distance computation (AVX2, AVX512, NEON). RaBitQ's error bounds let a search rule out candidates without reranking them against full-precision vectors. Reads are mostly sequential rather than the random reads HNSW needs.

## Practical details

- Supports inserts, updates and deletes, with posting-list splits via `prism_rebalance`.
- Works with its own `vec32`/`vec16` types, or directly on existing pgvector `vector` and `halfvec` columns.
- Uses the usual operators (`<->`, `<#>`, `<=>`) and a `prism.nprobe` setting to trade recall for speed.
- Alpha: upgrades between prereleases may require dropping and recreating the extension and its indexes.
