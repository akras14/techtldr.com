---
title: "DuckDB 2.0 LTS adds server mode, async I/O, VARIANT and triggers"
slug: "duckdb-2-0-lts-release"
date: 2026-10-21T15:04:01+0000
summary: "DuckDB 2.0.0 \"Cinnamon Teal\" is a long-term support release built from nearly 20,000 commits since 1.5. It adds a client-server mode, async I/O for object storage, a first-class VARIANT type, a PEG-based SQL parser, triggers and a CLI agent mode. The new storage format can't be read by pre-2.0 versions, and the team suggests waiting for the November patch release for mission-critical systems."
source: "https://duckdb.org/2026/10/21/announcing-duckdb-200"
source_title: "Announcing DuckDB 2.0.0 LTS \"Cinnamon Teal\""
source_author: "The DuckDB team"
source_site: "duckdb.org"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50181136"
---

DuckDB 2.0.0 is an LTS release and the project's largest feature release so far, supported until at least October 2027. Earlier hands-on numbers for the alpha are in [this benchmark post](/duckdb-2-faster/).

**New**
- **Quack protocol and server mode:** a `CONNECT` statement plus `-serve`/`-connect` CLI flags. `CONNECT` also works with PostgreSQL and MySQL, with a remote pushdown optimizer.
- **Async I/O:** background Parquet and CSV reads with multiple requests in flight, aimed at object storage like S3.
- **VARIANT type:** common structure is detected and shredded for better compression and speed, with `variant_*` functions.
- **CLI agent mode:** when a coding agent drives the CLI it prints compact Markdown tables, JSON errors and expected query cost. In TPC-H tests this cut the output the model had to read by 59%.
- **PEG-based SQL parser** (now default), enabling extensions that add syntax, with source locations in errors.
- **Triggers** (BEFORE/AFTER, row and statement level), data-modifying CTEs, `NEAREST` joins, nested schemas, JSON mutation functions and more. ICU is gone; timezones and collations are built in.
- **Extensions:** self-hosted signed extension repositories, C API v2, DuckLake 1.1, Iceberg REST catalog options, and Delta `CREATE TABLE`, time travel and VARIANT support.

**Breaking changes**
- Pre-2.0 versions can't read files created by 2.0 (2.0 reads and writes older files).
- C++17 required; Clang is the default release compiler.
- Single-arrow lambdas (`x -> x + 1`) are off by default and removed in 2.1. Amalgamation builds are deprecated. macOS x86_64 support ends in 2.1.

Support: 1.4 until late November 2026, 1.5 extended to March 2027. Test on a shadow system first; mission-critical users may prefer waiting for the November patch release.
