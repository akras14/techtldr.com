---
title: "Torvalds posts a 10-patch LLM-assisted series, 10x faster read() via RCU"
slug: "torvalds-llm-assisted-folio-patches"
date: 2026-10-14T18:03:00+0000
summary: "Linus Torvalds has posted his first patch series written with LLM help, after mentioning it at the Maintainers Summit. The ten patches mark hot folios for delayed release after an RCU period, so read() fast paths can touch folio data under RCU without taking refcounts. LWN reports an order-of-magnitude speedup on a read()-heavy benchmark, but review has already found a race, so the series needs more work."
source: "https://lwn.net/Articles/1100449/"
source_title: "Linus Torvalds's first LLM-assisted patch series"
source_author: "Jonathan Corbet"
source_site: "LWN.net"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50090842"
---

**Bottom line:** At the Maintainers Summit, Linus Torvalds said he had been using a large language model on a long-standing performance problem. He has now posted the result for review: a short series of ten patches to folio lookup in the Linux kernel.

## What the patches do

- They detect "hot" folios and delay releasing them until after an RCU grace period.
- That allows not just the folio lookup (already done under RCU) but also the reads of the folio data itself to happen inside an RCU-protected region.
- On the fast path, where the user copy doesn't page-fault, no refcount needs to be taken.
- LWN says this gives an order-of-magnitude speedup on a `read()`-heavy benchmark.

## Caveats

- Reviewers have already identified a race condition, so the series is not ready to merge.
- The LWN comments show the usual split over whether a maintainer using an LLM sets a bad example while the kernel is tightening rules for contributors. The article itself doesn't take a side.
