---
title: "Jepsen found Orkes Conductor abandoning and losing workflows"
slug: "jepsen-orkes-conductor"
date: 2026-10-20T20:02:48+0000
summary: "Jepsen tested Orkes Conductor 5.3.2 through 5.5.0-rc2 on Postgres and Redis and found three serious safety bugs: workflows permanently abandoned, workflows forgotten after being visible, and workflows marked completed without running their tasks. Orkes patched all of them in 5.5.6."
source: "https://jepsen.io/analyses/orkes-conductor-5.3.2"
source_title: "Jepsen: Orkes Conductor 5.3.2"
source_author: "Kyle Kingsbury"
source_site: "jepsen.io"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50165904"
---

**Bottom line:** Jepsen, funded by Orkes, tested the workflow orchestrator Orkes Conductor (originally built at Netflix) with Postgres as the database and Redis as the queue, under process kills, pauses and network partitions. It found three serious problems that made Conductor abandon or forget workflow executions, or mark them completed though their tasks never ran. Orkes shipped 5.5.6 with fixes for all the serious safety issues.

- **Abandoned workflows:** if a failure hit between writing a workflow to Postgres and enqueuing it on the decider queue, the workflow stayed RUNNING forever. The sweeper checked only the queue, never the database. Orkes added a re-enqueue process in 5.5.0.
- **Lost workflows:** a partition from Postgres could make a server roll back a workflow after clients had already seen it. Afterwards lookups returned null or 404, permanently.
- **Concurrent execution:** the same task ran on two workers at once, about once every six hours in a healthy cluster at roughly 1,100 workflows per second, and every few minutes with a one-second lock lease. A time-of-check/time-of-use bug in the sweeper made it worse. Fixed in 5.5.6, and Jepsen suggests documenting that tasks may run concurrently.
- **Smaller issues:** a server fault could stall workflows for the default one-hour response timeout (Orkes chose not to change this); startWorkflow sometimes returned null IDs, a spurious SQL syntax error (a partition-rotation race) or a duplicate-key error. Failed-looking startWorkflow calls sometimes had actually succeeded, because the Java client silently retried non-idempotent requests; Orkes changed the client.

Jepsen notes the research was funded by Orkes and followed its ethics policy.
