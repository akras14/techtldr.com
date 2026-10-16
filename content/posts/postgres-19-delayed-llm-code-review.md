---
title: "Postgres 19 slips to November after LLM code review surfaces dozens of defects"
slug: "postgres-19-delayed-llm-code-review"
date: 2026-10-16T17:03:20+0000
summary: "PostgreSQL 19 is delayed to about November 19 and 8–10 features were reverted to PostgreSQL 20. Committer Peter Eisentraut says LLM-assisted code review raised the effective quality bar after the features were written, turning up many small edge-case defects that would once have surfaced years after release."
source: "https://www.theregister.com/databases/2026/10/16/postgres-19-delayed-after-ai-reviewers-find-more-bugs-than-humans-can-fix/5303471"
source_title: "Postgres 19 delayed after AI reviewers find more bugs than humans can fix"
source_author: "Lindsay Clark"
source_site: "The Register"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50119991"
---

PostgreSQL 19 was due in late September or early October. It is now expected around November 19, and between eight and ten significant features have been reverted and pushed to PostgreSQL 20.

Peter Eisentraut (EDB, a leading committer) attributes this at least partly to LLM-assisted code review. Some reverted features had architectural defects that couldn't be fixed quickly during beta. But the reviews also found a long tail of mostly harmless edge-case bugs, which would previously have been found much later and fixed in the five years after release. Even at three lines per fix, each has fixed overhead, and "you run out of time."

His argument is that the effective quality bar rose after the features were committed, since LLM review only became useful in the months before release. He says it was not vibe coding, because that wasn't available when most of the code was written. He expects the problem to fade in future cycles unless LLM review keeps improving at the current rate.

- SQL Property Graph Queries (SQL/PGQ) was among the features pulled. Tom Lane had warned he'd bet dinner that shipping it would mean post-release bugs unfixable until v20.
- REPACK CONCURRENTLY, which reclaims disk space without the table lock of VACUUM FULL, stays in 19.
- PostgreSQL 20 was expected to be a bug-fix-only release. It will now carry the reverted features.
