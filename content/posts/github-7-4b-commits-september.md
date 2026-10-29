---
title: "GitHub hit 7.4B commits in September, up from 65M a month in 2024"
slug: "github-7-4b-commits-september"
date: 2026-10-29T01:02:37+0000
summary: "GitHub now sees 7.4 billion commits a month, driven mostly by AI agents, up from 2.9 billion in August and 1.4 billion in April. Developers pushed about 65 million a month in 2024. To keep up this year it has added 4 million CPU cores (mostly on Azure), 160 PB of fast storage, more Actions compute and 50 Tb/s of network capacity, and it is rearchitecting to separate compute from storage."
source: "https://thenewstack.io/github-7-4b-commits-september/"
source_title: "GitHub saw 7.4B commits in September, adding 4M CPU cores and 160 PB of storage to keep up"
source_author: "Frederic Lardinois"
source_site: "The New Stack"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50279722"
---

GitHub's commit volume has grown by roughly two orders of magnitude in two years, and the company attributes it to "agentic demand". Announced at GitHub Universe: 7.4 billion commits in September, against 2.9 billion in August and 1.4 billion in April.

- **Capacity added this year:** 4 million CPU cores (an August update said 3 million, mostly on Azure), 160 petabytes of high-speed storage, more Actions compute, and 50 Tb/s of network capacity.
- **Architecture change:** GitHub is separating compute from storage. Its internal benchmarks show up to 35x higher write throughput, it says. Microsoft's Jay Parikh: capacity alone won't get there, the architecture had to change.
- **Context:** the article notes GitHub's outages have become a recurring theme, and that separating compute and storage isn't radical in 2026. It reads as GitHub scaling on a legacy foundation, with Azure close to the only option because its own data center was out of capacity.

Related earlier post: [GitHub's per-reference quorum](/posts/github-git-per-reference-quorum-azure/) covers one piece of the storage work.
