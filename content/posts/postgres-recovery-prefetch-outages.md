---
title: "Postgres recovery_prefetch can turn 1 replica lag burst into hours-long write outages"
slug: "postgres-recovery-prefetch-outages"
date: 2026-10-22T08:03:00+0000
summary: "Jeremy Schneider shows that Postgres's recovery_prefetch setting can amplify replica read IOPS by up to 16x, because it turns the primary's large sequential I/O into 8k random reads. Combined with synchronous replication, a write burst plus a brief walreceiver disconnect left his test cluster with zero TPS for over 30 minutes. He is leaning toward disabling recovery_prefetch everywhere."
source: "https://ardentperf.com/2026/10/20/postgres-recovery_prefetch-considered-harmful-and-causing-outages/"
source_title: "Postgres recovery_prefetch Considered Harmful and Causing Outages"
source_author: "Jeremy Schneider"
source_site: "Ardent Performance Computing"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50188614"
---
**Bottom line:** Jeremy Schneider found a production failure mode in Postgres's `recovery_prefetch` (the WAL-replay prefetching feature). When standbys fall behind after a burst of writes, the feature can exhaust the replica's disk IOPS, and with synchronous replication that stalls writes on the primary for hours.

**What goes wrong**
- Earlier mailing-list posts (2021 during development, 2025 from a Postgres Pro customer) flagged that prefetching ignores blocks that are restored from full-page images (FPIs), causing needless `fadvise` calls. Schneider says the bigger problem was not yet diagnosed.
- Recovery prefetch converts the primary's large sequential I/O into 8k random reads on the replica. For the same number of bytes read, that can amplify IOPS up to 16x.
- A standby that drops into WAL-restore mode doesn't go back to streaming until it has caught up. With IOPS exhausted, catching up can take hours.
- With synchronous replication, the primary can't commit without a healthy standby, so all writes block.

**Reproduction:** on small servers (2 CPUs, 8GB RAM, a 500 IOPS cloud disk) with synchronous replication, run pgbench at scale 640 at a fixed 5 TPS, update 5% of `pgbench_accounts`, then kill the walreceiver process. TPS falls to zero for over 30 minutes, and longer with bigger tables. A repro repo is linked from the post. His charts show the primary at about 300 IOPS while the replica sits at its 500 IOPS limit for 40 minutes, with about the same bytes read.

**Fix and mitigation:** with `recovery_prefetch=off` (full-page writes still on), there was no read-IOPS burst and the standby stayed caught up, applying the update in under 10 minutes. Schneider is leaning toward disabling it across the board, and says the discussion on the Postgres lists is still open. He also suggests two standbys with quorum commit (`ANY 1`), which makes the setup more robust at the cost of a third machine.
