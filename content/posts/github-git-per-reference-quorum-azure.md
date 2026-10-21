---
title: "GitHub's per-reference quorum gives 3–4x write throughput across Azure"
slug: "github-git-per-reference-quorum-azure"
date: 2026-10-21T19:05:00+0000
summary: "GitHub is moving Git storage to Azure and has hit a wall on its busiest repositories: a 25ms cross-region round trip, multiplied by a repository-wide write lock. It is changing Spokes' quorum to serialize writes per reference rather than per repository, which gave 3–4x higher throughput at lower latency in internal tests, and adding ephemeral 'virtual' replicas backed by Azure Blob Storage so reads scale independently of writes."
source: "https://github.blog/engineering/architecture-optimization/the-bridge-to-agent-scale-git-infrastructure/"
source_title: "The bridge to agent-scale Git infrastructure"
source_author: "Eleanor White and Victoria Dye"
source_site: "github.blog"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50183344"
---

GitHub says agent-driven traffic, a hard power ceiling in its own data centers and supply-chain limits forced a fast move to Azure. Most repositories now keep two of their five replicas there, and Azure serves about half of Git traffic. The first replica landed in August 2025.

The holdouts are the busiest repositories. Spokes, GitHub's quorum layer, waits for every healthy replica before moving on, and each write holds a repository-wide lock. A replica about 1,000 miles away adds roughly 25ms per round trip, and a write needs several, so those repositories fall behind on every push.

- **Per-reference serialization:** Git only needs ordering per reference, so the quorum is now scoped to individual refs. Writes to different branches (agents and developers pushing to their own) no longer queue behind each other. Internal tests showed 3–4x throughput at lower latency for writes to different refs.
- **Virtual write replica:** object storage joins the quorum through a fully ephemeral node. A write isn't acknowledged until it is durable in Azure Blob Storage. It coalesces writes on disjoint refs into one set of I/Os and currently handles compaction.
- **Virtual read replicas:** these warm their cache from blob storage and serve reads without joining the quorum. Most repositories see 100 to 1,000 reads per write, and GitHub's load harness hit its own limits before the replicas'. Reconciliation is still in the background, so they don't yet suit every kind of read.

The quorum is still the limit on write throughput. GitHub says a final post, coming next week around GitHub Universe, will describe the end state.
