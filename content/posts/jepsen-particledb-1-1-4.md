---
title: "Jepsen found 96 issues in ParticleDB, including lost committed writes"
slug: "jepsen-particledb-1-1-4"
date: 2026-10-31T19:02:27+0000
summary: "Kyle Kingsbury's Jepsen analysis of ParticleDB, a Rust multi-model database for AI workloads, found 96 issues across versions 1.1.4 to 1.1.18.11.rc-22, most reproducible with a single server and no faults. By the last tested version all but ten appeared fixed, but it still lost committed writes in the default occ mode and showed Lost Update in serializable mode. Its \"Full ACID\" claim was premature."
source: "https://jepsen.io/analyses/particledb-1.1.4"
source_title: "Jepsen: ParticleDB 1.1.4"
source_author: "Kyle Kingsbury"
source_site: "Jepsen"
source_date: "2026-10-31"
hn_url: "https://news.ycombinator.com/item?id=50316100"
---

**Bottom line:** Jepsen tested ParticleDB, made by Orkes (makers of Conductor), and found its "Full ACID" marketing premature. The 96 issues ranged from crashes, deadlocks and data loss to odd ones like partially invisible rows that couldn't be deleted, `WHERE 4 = 5` misbehaving, dropped tables reappearing in distorted form, and strings turning into integers. The research was funded by Orkes.

## Findings

- **Fixed vs. not:** by 1.1.18.11.rc-22 all but ten issues appeared resolved. It still lost committed writes in normal operation under the default `occ` transaction mode, and allowed internal anomalies and Lost Update under `serializable`, the strongest setting. Orkes says 1.1.19-rc.13 fixes the rest; Jepsen did not verify that.
- **Unclear guarantees:** isolation is set by a `--txn-mode` flag at startup, while `SET TRANSACTION ISOLATION LEVEL` is ignored or rejected. Docs, `--help` and an unpublished operator guide disagreed. The operator guide says `occ` and `zero-copy-occ` are Snapshot Isolation, `serializable-ssi` is Serializable, and `table-2pl` is Strong Serializable.
- **Marketing vs. reality:** the homepage said "Full ACID" next to "snapshot isolation", and claimed to be "formally proven" with TLA+, but no spec was available during the work.
- **Durability:** docs disagreed on whether the `groupsync` WAL mode is as durable as `sync`.
- **Scope:** only single-node behavior was tested, since the provided builds had no replication (Raft clustering is enterprise-cloud only).
