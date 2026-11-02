---
title: "PostHog moves its 2 PB warehouse off DuckDB and DuckLake to Trino"
slug: "posthog-leaves-duckdb-trino"
date: 2026-11-02T16:35:03+0000
summary: "Five months after rebuilding its customer warehouse on DuckDB instead of ClickHouse, PostHog is replacing it with its own flavor of Trino and a new table format called HogLake. Single-node DuckDB, the DuckLake catalog and Postgres wire compatibility all stopped scaling as customer workloads grew."
source: "https://posthog.com/blog/moving-off-duckdb"
source_title: "Why we're moving our warehouse off DuckDB"
source_author: "Benjamin Knofe-Vider"
source_site: "PostHog"
source_date: "2026-11-02"
hn_url: "https://news.ycombinator.com/item?id=50338747"
---
**Bottom line:** PostHog gave each customer a DuckDB instance for its data warehouse, but customer queries grew bigger and more complex than expected. After three months of re-architecting, it is rebuilding on Trino with its own open table format, HogLake. The authors say they are still fans of DuckDB.

**Why DuckDB stopped fitting**
- **Single node:** the worker has to be sized before a query arrives, and predicting CPU and memory for a specific query was hard. That meant huge instances. Adding disks would have made queries too slow and costly. Async fetching from S3 only arrived in DuckDB recently.
- **DuckLake:** the catalog schema must stay portable across SQL databases, which limits optimization. At PostHog's scale a single-table write could take over three minutes because each commit re-read statistics for the whole catalog. Over 99% of statistics rows belonged to dropped tables; cleaning them made writes 30 to 50 times faster, but the rows grew back. It also opened many connections to shared Postgres metadata databases. The catalog held about 2 PB, roughly 1 trillion rows and 15.3 million snapshots. Changes could not land upstream fast enough, so PostHog carried patches in a fork.
- **Postgres wire protocol:** serving port 5432 was harder to scale than HTTP. Reconnects, TTLs and idle connections were painful, load balancing tools are weak, and customers kept hitting client-specific quirks.

PostHog says it is testing the new Trino and HogLake stack at its own petabyte scale first, before giving it to design partners.
