---
title: "Logfire's dynamic JSON shredding cuts some queries from 30s timeouts to under 1s"
slug: "logfire-dynamic-json-shredding"
date: 2026-10-12T17:03:09+0000
summary: "Pydantic's Logfire stored OpenTelemetry span attributes as one JSON column in Parquet, so any query on an attribute had to download and parse the whole blob. Dynamic shredding extracts each file's largest keys (default top 128) into their own typed columns at ingest and compaction, so queries read only what they need and can use zone-map pruning. Pydantic reports over 1,000x speedups in some cases and began rolling it out in February 2026."
source: "https://pydantic.dev/articles/dynamic-shredding-2026-01-26"
source_title: "How shredding JSON is giving Logfire 1000x query speedups"
source_author: "Adrian Garcia Badaracco"
source_site: "Pydantic"
source_date: "2026-01-29"
hn_url: "https://news.ycombinator.com/item?id=50048490"
---

**Bottom line:** Logfire sped up queries on semi-structured attributes by storing frequently used JSON keys as their own typed Parquet columns, chosen automatically per file. Pydantic says some queries that timed out at 30 seconds now finish in under a second.

- **The problem:** span and resource attributes were stored as a single JSON string column, parsed with the fast Jiter parser. Faster parsing didn't help, since the real cost was I/O: downloading large blobs from object storage, plus poor compression because attribute shapes vary.
- **First attempt, static shredding:** a hardcoded set of attributes (HTTP status code, method, URL, DB query text) got typed columns. Anything else, such as `user_id`, still paid the full JSON cost, and the internal `_lf_attributes_*` columns leaked into user-visible results.
- **Dynamic shredding:** during ingestion and compaction, each Parquet file extracts its largest keys by total size, defaulting to 128, since 99% of files have fewer than 128 distinct top-level keys (median about 32). The cap keeps junk such as thousands of random-UUID keys from exploding the column count. Queries written as `attributes->>'key'` are rewritten to hit the shredded column, gaining column pruning, zone maps, better compression and no JSON parsing.
- **Upstream work:** about six months of DataFusion changes to push expressions and predicates into the Parquet reader, landed in DataFusion 52.1.0, with GROUP BY pushdown expected the following month.
- **Rollout:** the query path was switched first while the write path mimicked the old static configuration, and only then did the write path switch to the discovery algorithm.
