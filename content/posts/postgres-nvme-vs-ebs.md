---
title: "Local NVMe ran a Postgres update workload 9.24x faster than gp3 EBS"
slug: "postgres-nvme-vs-ebs"
date: 2026-10-12T09:02:24+0000
summary: "In a ClickHouse recap of a POSETTE 2026 talk, the same random-UPDATE workload on a 3.3-billion-row (482 GiB) pgbench table hit a median 16,030 TPS on local NVMe versus 1,734 TPS on baseline gp3 EBS (3,000 IOPS), with median latency of 4.0 ms versus 36.9 ms. Most of the EBS gap was time waiting on storage: page reads took about 19 ms versus 0.3 ms and WAL fsync about 11 ms versus 1.5 ms, while CPU work was about 2 ms in both. VACUUM over 10 GB of bloat took 366 s versus 964 s, and draining a 10 GB logical-replication backlog took 139 s versus 238 s. The results apply to this workload and this EBS configuration, and the post is from a vendor that sells Postgres. Because local NVMe dies with the node, the talk proposes quorum synchronous replication across two standbys in different availability zones, plus WAL-G base backups and WAL archives stored outside the nodes."
source: "https://clickhouse.com/blog/posette-talk-recap-postgres-isnt-slow-your-storage-is"
source_title: "POSETTE Talk Recap - Postgres Isn't Slow. Your Storage Is"
source_author: "ClickHouse"
source_site: "clickhouse.com"
source_date: "2026-08-20"
hn_url: "https://news.ycombinator.com/item?id=50052163"
---

Summary above; see the source for the full benchmark setup (eight identical m6id.4xlarge clusters, four per storage type) and the production architecture.
