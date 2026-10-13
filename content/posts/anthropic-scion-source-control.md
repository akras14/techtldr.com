---
title: "Anthropic replaced git with Scion after merges grew from 2,500 to 30,000 a day"
slug: "anthropic-scion-source-control"
date: 2026-10-13T18:02:51+0000
summary: "Anthropic built Scion, a server-centric source control system, to replace git mirrors for its monorepo after daily merges grew from about 2,500 (May 2026) to over 30,000 (September). Scion's footprint is about 200x smaller than the old mirror setup, CI checkouts take seconds instead of minutes, and CI gained roughly 25% CPU capacity. It's internal-only and tied to Anthropic's tooling."
source: "https://www.anthropic.com/engineering/redesigning-source-control"
source_title: "Redesigning Source Control at Anthropic"
source_author: "Nick G. and the Anthropic source control team"
source_site: "Anthropic"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50073633"
---

**Bottom line:** Anthropic says coding is no longer its bottleneck; developer infrastructure is. With agents driving merges from about 2,500 a day in May 2026 to over 30,000 by September, git mirrors (several hundred at peak) became too hard to run. Seven engineers built a replacement, Scion, and finished the migration in five months.

**The scale problem.** The monorepo's main history is nearly 100 GB, and a compressed working copy exceeds 25 GB. About 25 million CI checkouts a day would mean over 600 PB of git traffic if fetched naively.

**Core idea.** Git is designed for distributed, offline work, but nearly all of Anthropic's clients are always-online agents and jobs. Scion drops those assumptions:
- The server owns history, blame, merges and rebases; the client keeps a bounded cache and a sparse, virtualized working copy, paying only for files it reads.
- The unit of work is a server-side changeset in a write-ahead log, listing files changed against its first parent. Refs move by compare-and-swap on a single Spanner row. There are no packfiles and no repo maintenance.
- Derived data (blame, path history, commit-graph skip lists, search indices) is computed by one generic recursive mechanism, eagerly or lazily.
- The fetch protocol asks only for needed trees and blobs, is resumable, and never sends multi-gigabyte responses. Servers, regional caches and a per-node daemon speak the same RPCs, with strict tiering so a cache failure can't overwhelm upstream.

**Results.** Each tier holds a cache hit rate above 99%. The footprint is about 200x smaller than the old mirrors, checkouts take seconds rather than minutes, and CI gained about 25% CPU capacity.

**Working with agents.** Engineers set architecture and tradeoffs, and often rejected agent-proposed patterns that broke system constraints, such as scanning commit logs linearly. Adversarial review sometimes over-engineered code. The post says an early Claude Mythos Preview snapshot rewrote the hand-built git-to-Scion ingestion pipeline unprompted, using 20x less memory and running 4x faster. Per-component "superfan" agents watch telemetry and logs for issues alerts miss. A global merge lock is impractical at this volume, so the team leans on post-merge and pre-deploy testing.

**Caveats.** Scion isn't a general git replacement, suits only enormous monorepos, is coupled to internal tooling, and won't be released. The authors expect many organizations to hit similar scaling limits within 6 to 12 months and hope git platforms adopt server-side RPCs.
