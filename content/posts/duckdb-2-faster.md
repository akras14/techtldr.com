---
title: "DuckDB 2.0 alpha: S3 reads 2-3x faster, deep recursive CTEs far faster"
slug: "duckdb-2-faster"
date: 2026-10-10T20:02:47+0000
summary: "In hands-on tests of the DuckDB 2.0 alpha on a laptop, Mehdi Ouazza finds big gains from three features: async I/O makes S3 Parquet reads 2-3x faster with no query changes, a rewritten recursive CTE engine cuts a 20,000-commit ancestry walk from up to 16 s to 0.1 s, and shredded VARIANT is 2.7x smaller than JSON text and about 6x faster on field queries."
source: "https://motherduck.com/blog/why-duckdb-20-is-faster/"
source_title: "Why DuckDB 2.0 is faster"
source_author: "Mehdi Ouazza"
source_site: "MotherDuck"
source_date: "2026-09-10"
hn_url: "https://news.ycombinator.com/item?id=50035530"
---
All numbers come from one M5 laptop and a home internet connection, and the author asks readers to rerun them before quoting.

**Async I/O.** A separate pool of threads now downloads row groups ahead of the decoding workers, so network and CPU work overlap. Reading one column of a 2.2 GB Parquet file on S3 dropped from 18.8 s (1.5.5) to 7.7 s; 23 large files went from 11.8 s to 3.9 s; a 1.7 GB CSV from 116 s to 55 s. It is on by default (`read_ahead_depth = -1`; 0 restores the old behaviour). Many tiny Parquet files barely improve.

**Recursive CTEs.** The old engine re-read the whole table each round, so deep hierarchies were slow. The new one builds a parent lookup once and touches only the rows each round finds. Walking a 20,000-commit history took 1.8-16 s in 1.5.5 and about 0.10 s in 2.0. Shallow hierarchies such as org charts won't see much change.

**VARIANT.** DuckDB now shreds consistently typed fields of semi-structured data into real columns, leaving the messy rest in a binary remainder. For 5 million events, storage fell from 224 MB (JSON string) to 85 MB, and filter and sum queries ran about 6x faster than on JSON text and within 20% of fully typed columns. List casts are still slow in the alpha (about 2 s), and fields whose value type varies between rows fall into the remainder. The advice: promote fields every query touches to real columns.

The post also lists smaller additions: triggers with transition tables, nested schemas, and DML inside CTEs.
