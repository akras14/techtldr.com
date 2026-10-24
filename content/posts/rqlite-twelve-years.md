---
title: "Replicated SQLite has been around since 2014, says rqlite's author"
slug: "rqlite-twelve-years"
date: 2026-10-24T04:02:10+0000
summary: "Philip O'Toole, who released rqlite in 2014, argues replicated SQLite is a twelve-year-old idea that at least eight teams have independently built, not a 2021 invention. He sees three design corners that appeared in order: consensus (rqlite, dqlite), streaming (Litestream) and merge (cr-sqlite), each suited to different problems."
source: "https://philipotoole.com/twelve-years-of-distributed-sqlite/"
source_title: "Twelve years of distributed SQLite"
source_author: "Philip O'Toole"
source_site: "philipotoole.com"
source_date: "2026-10-23"
hn_url: "https://news.ycombinator.com/item?id=50217500"
---

**Bottom line:** Philip O'Toole, who released rqlite in September 2014, says replicated SQLite is a twelve-year-old idea, not something that began around 2021. He thinks rqlite was the first open-source system to put Raft in front of SQLite, and that the idea has since been independently reinvented by at least eight teams.

## Timeline

- **2014, rqlite:** a Raft log in front of SQLite replicates every write. It is a CP system: writes go through the leader, a majority must acknowledge them, and a partitioned minority refuses writes. It moved to HashiCorp's Raft library in 2016.
- **Before and alongside:** Expensify's Paxos-style replication (released as Bedrock in 2016) and ActorDB, which gave each actor its own Raft-replicated SQLite database.
- **2017, dqlite:** Canonical's embedded library for LXD and MicroK8s, built to avoid running etcd. Rewritten from Go to C with its own Raft.
- **2021, Litestream:** no consensus, just streaming the WAL to S3 for cheap single-node disaster recovery.
- **2022 onward:** LiteFS (Fly.io), cr-sqlite (CRDTs), libSQL and Turso (the author says Supabase acquired it in 2026), SQLite Cloud, and Cloudflare D1 and Durable Objects.

## Design space

O'Toole sees three corners, which appeared in this order:

1. **Consensus** (rqlite, dqlite): one leader, strong consistency, a cluster.
2. **Streaming** (Litestream): one writer, cheap recovery, no cluster.
3. **Merge** (cr-sqlite, local-first): many writers, eventual consistency.

Many debates, he says, come down to which problem is being discussed.

## Commercial angle

rqlite, dqlite and Litestream remain free and open source. The projects that raised money or were acquired turned replicated SQLite into a paid service, after eight years in which nobody sold it. O'Toole accepts that as a normal path for infrastructure and positions rqlite as the choice for a fault-tolerant relational database you can run yourself without a vendor.
