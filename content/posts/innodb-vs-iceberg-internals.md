---
title: "Moving a 100GB stock database from InnoDB to Iceberg: what changed inside"
slug: "innodb-vs-iceberg-internals"
date: 2026-10-14T12:03:52+0000
summary: "After five years on MySQL/InnoDB, the author moved an analytics-heavy stock database to Iceberg on MinIO with Polaris and Spark, because analytics read only 2 or 3 columns per table but InnoDB's row storage reads them all. The post walks through how Iceberg prunes data at three levels (manifest list, manifests, Parquet row groups) and how its commits work compared with InnoDB's undo log and B-tree indexes."
source: "https://marcpaul.io/logs/same-job-two-engines-innodb-and-iceberg-internals/"
source_title: "Same Job, Two Engines: InnoDB and Iceberg Internals"
source_author: "Marc Paul"
source_site: "marcpaul.io"
source_date: "2026-09-14"
hn_url: "https://news.ycombinator.com/item?id=50071466"
---

The author ran stock analytics on InnoDB for five years, then realized each indicator query needed only 2 or 3 columns while a row store reads whole rows. The v2 stack is Iceberg tables (Parquet files on MinIO) behind a Polaris catalog, read with Spark. The indicator table has 36 columns, and a query needing close and volume now reads those two and leaves the other 34 on disk.

**InnoDB (v1):**
- Rows store all columns together, which suits ingesting one bar at a time.
- Every index is its own B-tree that every write must update, so heavy indexing is bad for ingest. He kept raw tables primary-key-only.
- The undo log and MVCC let reads proceed during writes.
- His design had a base `FinancialData` table with indicator tables joined on its IDs, which he calls one of his worst data designs and only noticed past 100GB. Backfilling dates into the indicator tables was painful, and repartitioning a hash-partitioned table took days to a week.

**Iceberg (v2):**
- Partitioning is metadata, so the scheme can change later. Data is pruned per partition version and the results merged ("split planning").
- Reads prune through three nested layers of statistics: the manifest list (partition ranges per manifest), manifests (min/max and null counts per file, plus delete files for merge-on-read) and Parquet row groups.
- Polaris is a Postgres-backed catalog whose main job is holding a pointer to each table's current `metadata.json`.
- Writes create new immutable files and swap that pointer atomically. A losing writer re-applies its changes on the new snapshot if no files overlap, otherwise it fails and retries. Readers keep the snapshot they started with, which replaces the undo log's job.

He notes the same stats-at-write, prune-before-read model underlies Snowflake, BigQuery and Delta Lake, and says learning it took about nine months. Open items he lists: `WRITE ORDERED BY` isn't set up yet, and old `metadata.json` files aren't cleaned up.
