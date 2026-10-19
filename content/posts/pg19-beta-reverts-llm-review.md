---
title: "PostgreSQL 19 reverted 8–10 features in beta, partly because LLM review found too many bugs"
slug: "pg19-beta-reverts-llm-review"
date: 2026-10-19T20:03:00+0000
summary: "PostgreSQL committer Peter Eisentraut says about 8 to 10 significant features were reverted from the PostgreSQL 19 branch during beta, far above the usual one per cycle. His explanation is a mix of causes. The main one is that LLM-assisted code review and fuzzing now surface dozens of edge-case defects in code written before such tools were good, and the project chose to hold the release schedule rather than slip."
source: "http://peter.eisentraut.org/blog/2026/10/09/the-reverts-will-continue-until-morale-improves"
source_title: "The reverts will continue until morale improves"
source_author: "Peter Eisentraut"
source_site: "peter.eisentraut.org"
source_date: "2026-10-09"
hn_url: "https://news.ycombinator.com/item?id=50156206"
---

PostgreSQL 19 lost roughly 8 to 10 significant features to reverts after beta began, some very late. Normally a cycle sees about one. Eisentraut, who authored or committed some of the reverted work, offers his own opinions rather than an official project position.

His read is that several factors combined, differently per feature:

- **Higher effective quality bar.** LLM-assisted review now finds a long tail of edge-case bugs that previously surfaced slowly over the following years. Dealing with a list of around forty reports, each a small fix, has fixed overhead that ran out the clock. He says this is mostly not "vibe coding", since the code predates it.
- **Reduced review time.** Senior developers spent roughly April to August on security fixes (the "Vulnpocalypse"), so the usual beta-period review and testing was thinner and then resumed late.
- **Complexity.** Because "everything works with everything" in Postgres, feature interactions explode (domains over composites, detached partitions, security-definer triggers). He argues LLM-assisted fuzzing should become part of feature development and internal interfaces should be made more robust.
- **Schedule over slip.** The community chose to keep the release date instead of delaying a couple of months.

He also points to a possible "experimental" status for features committed to the main tree as an unresolved idea. He expects many reverted features to return and says PostgreSQL 19 will still ship REPACK CONCURRENTLY, logical replication of sequences and planner hinting with pg_plan_advice.
