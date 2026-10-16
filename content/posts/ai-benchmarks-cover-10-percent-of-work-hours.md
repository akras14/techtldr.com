---
title: "AI benchmarks cover only 208 of 923 occupations, about 10% of US work hours"
slug: "ai-benchmarks-cover-10-percent-of-work-hours"
date: 2026-10-16T18:02:49+0000
summary: "Mercor mapped 29 public agentic benchmarks onto the US Labor Department's O*NET task list and estimates that about 90% of American working hours go to tasks with little or no benchmark coverage. Only 208 of 923 occupations have at least one task with five or more benchmark prompts, and six of 22 job families have none, so how well models handle most real work is largely unmeasured."
source: "https://www.mercor.com/research/labor-index/benchmark-map/"
source_title: "A benchmark map of the US economy"
source_site: "Mercor"
hn_url: "https://news.ycombinator.com/item?id=50121179"
---

**Bottom line:** Benchmarks are concentrated in a few kinds of work. Mercor estimates roughly 90% of US working hours go to tasks that existing AI benchmarks barely test.

## What they did

- Used O\*NET (18,797 tasks across 923 occupations in 22 job families) as the taxonomy.
- Picked 29 public benchmarks: at least 25 public tasks, agentic rather than Q&A, economically realistic, diverse, and not yet saturated by frontier models.
- Had an LLM judge tag each benchmark prompt with the O\*NET tasks it matches (validated against human tags on Mercor's APEX), and called a task "covered" if 5 or more prompts map to it. Only 924 tasks qualified.
- Weighted coverage by estimated time spent on each task to get a share of working hours.

## Findings

- Only 208 of 923 occupations have a covered task; six job families have none.
- Tasks rated "exposed" to AI in earlier research are about 6x more likely to be benchmarked than unexposed ones.
- Even after controlling for exposure, occupations with larger wage bills are overrepresented. Lawyers' tasks are heavily covered; legal secretaries and court clerks barely are.
- Within an occupation, the most important tasks are 6-10x more likely to be benchmarked than the least important.
- Computer and math work (29 of 36 occupations) and business and finance (33 of 48) are best covered; healthcare practitioners (12 of 89) and education (9 of 62) are among the worst. Software engineering and financial analysis are heavily benchmarked, truck driving and gas plant operation are sparse.
- A pilot weighting APEX scores by O\*NET tasks changed occupation scores only modestly (investment banking: 73.3% simple average vs 70.2% weighted for one model), though fewer than a third of relevant tasks had enough prompts.

## Caveats

The authors call the results provisional. The benchmark list is incomplete, per-family caps understate coverage in heavily benchmarked fields, excluding saturated benchmarks may understate coverage of easier tasks, and "economic realism" is a judgment call. Benchmarks also mostly measure full automation, while people mostly use AI as an aid, and physical work is hard to capture at all. Mercor sells expert-data services for AI evaluation and built some of the benchmarks used.
