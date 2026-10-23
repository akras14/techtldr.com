---
title: "Metastable failures reproduced in real Postgres 16 and etcd, shown as animations"
slug: "metastable-failures-postgres-etcd-animations"
date: 2026-10-23T22:04:14+0000
summary: "Marc Brooker uses LLM-generated animations, backed by traces from real test environments, to show that metastable failures happen in widely used, production-proven software. A load spike pushes Postgres 16 out of its page cache, etcd into a storm of retries, and etcd's garbage collector into a loop that doesn't recover without intervention."
source: "https://brooker.co.za/blog/2026/10/23/animations.html"
source_title: "Animations for System Builders: Metastability and Lost Writes"
source_author: "Marc Brooker"
source_site: "Marc's Blog"
source_date: "2026-10-23"
hn_url: "https://news.ycombinator.com/item?id=50212124"
---
Brooker, an AWS engineer, argues that animations are a good way to build intuition for how systems fail, and that LLMs make them cheap to produce. His target is skepticism that metastable failures, where a temporary load spike leaves a system stuck in a bad state, occur in mature software. He shows three cases captured from real traces.

- **Postgres 16 (in a MicroVM):** At 20 queries per second the working set sits in the page cache. After a spike, queries (about 12MB each) fill memory and push data out of the cache. Queries then hit disk and slow down, which keeps the load high. Brooker notes this happens even in a simple, non-distributed system.
- **etcd, write overload:** A write spike makes overloaded peers look unavailable, which triggers a storm of application-level resends.
- **etcd, GC:** Raising reads from 500/s to 1000/s increases time in garbage collection and average object tenure, leaving less CPU for reads. The loop reinforces itself and doesn't recover without intervention. He suspects a runtime without GC could behave the same way through allocator work, but hasn't reproduced that.

He also mentions that a Raft cluster can lose writes if even one member's storage layer lies about `fsync()`. The animations were generated with Kiro, mostly using Opus 5.5, and are backed by traces from real test environments.
