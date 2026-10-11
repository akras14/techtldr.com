---
title: "sqlite-multiwriter: 5.7x write throughput on stock SQLite via a custom VFS"
slug: "sqlite-multiwriter-concurrent-writers"
date: 2026-10-11T21:02:27+0000
summary: "sqlite-multiwriter is an Apache 2.0 project that lets multiple threads and processes write to one SQLite database concurrently, with no SQLite patches and no file format change. In the author's benchmarks with full synchronous durability, 16 threads got 49,277 transactions per second versus 8,630 for WAL-mode SQLite (5.7x), and p99.9 latency dropped from 157 ms to 2.08 ms."
source: "https://marcobambini.substack.com/p/we-solved-sqlites-single-writer-limitation"
source_title: "We solved SQLite's single-writer limitation"
source_author: "Marco Bambini"
source_site: "marcobambini.substack.com"
source_date: "2026-10-09"
hn_url: "https://news.ycombinator.com/item?id=50044919"
---

**Bottom line:** Marco Bambini released sqlite-multiwriter, an open-source (Apache 2.0) SQLite Virtual File System that allows concurrent writers across threads and processes. It works on existing database files with unmodified SQLite. The motivation is AI agents: one agent may run dozens of parallel sessions against a single database.

**How it works**
- Each writer works on its own snapshot and writes to a private WAL, instead of competing for the single write lock.
- At commit, the engine validates the transaction against other committed ones and publishes it if compatible (first committer wins). A commit log and compaction maintain the database file.
- An optional rebase step can resolve some page-level conflicts, such as two transactions touching different rows on the same page.
- Real conflicts still require retries, and the project documents its limitations.

**Benchmarks (vs. WAL-mode SQLite, full synchronous durability)**

| | SQLite | sqlite-multiwriter |
|---|---|---|
| 16 threads, TPS | 8,630 | 49,277 (5.7x) |
| 16 threads, p99.9 latency | 157 ms | 2.08 ms |
| 16 processes, TPS | 8,636 | 25,987 (3.0x) |
| 16 processes, p99.9 latency | 233 ms | 1.54 ms |

**Caveats:** These numbers come from workloads where each writer inserts its own rows. Workloads with genuine same-row conflicts will behave differently. The author says more testing, workloads, and platforms are still needed.
