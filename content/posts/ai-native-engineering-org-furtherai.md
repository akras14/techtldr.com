---
title: "FurtherAI engineers merge 5x more changes a day, with reverts under 0.5%"
slug: "ai-native-engineering-org-furtherai"
date: 2026-10-30T20:02:31+0000
summary: "FurtherAI's engineering lead says that since January each engineer merges about 5x more code changes a day while the revert rate stayed under 0.5%. Coding agents alone didn't do it: the team rebuilt each step of shipping (tests, AI review, CI, bug intake, automated rule checks, auto-merge) because work piled up at whichever step was left slow. The new bottleneck is deciding what to build and reviewing it, so he is pushing planning and review ownership onto engineers and asking managers to be more product-savvy."
source: "https://twitter.com/sgondala2/status/2115832070274723875"
source_title: "Building an AI-native engineering org"
source_author: "Sashank Gondala"
source_site: "X"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50296730"
---

Over nine months at FurtherAI, Sashank Gondala reports that each engineer now merges about 5x more changes per day than in January, with the revert rate under half a percent. Customer-facing changes per engineer stayed flat at 17-30 a month through the summer while the team invested in tooling, then rose to 45 in September and are on pace for 48 in October.

## What changed in shipping

Giving engineers coding agents wasn't enough: each time one step sped up, work piled up at the next. So the team went through the pipeline step by step:

- **Code (March):** agents write almost all the code; engineers steer.
- **Tests (April):** 91% of changes include tests, up from 15%.
- **Review (May):** AI reviews 99% of changes before a person does, up from 60%.
- **CI (June):** parallelized and affected-only testing runs 20x the tests in the same 17 minutes.
- **Bug reports (July):** a Slack-based agent dedupes, files and assigns tickets, now 60% of bug tickets.
- **Rules (August):** tribal knowledge from bugs and review comments becomes small Python checks that fail the build and print the fix. There are 21 so far.
- **Merging (August):** low-risk changes that pass checks and AI review merge on their own, up to a third of all changes.
- **Code channels (September):** anyone can describe a change to an agent in Slack and get a PR; non-engineers, such as the designer with about 50 merged changes, now ship too.

## The new bottleneck

Writing code got faster, but deciding what to build did not. His example: a ten-day feature (3 planning, 6 building, 1 review) with building 3.5x faster only becomes 1.75x faster overall, and planning plus review go from 40% to 70% of the time.

His bet is more ownership for engineers rather than more management layers. He sketches seniority as: junior builds with an agent and can tell when it's wrong; senior plans and reviews their own work; staff takes a vague problem, makes the tradeoffs and owns the success metric. Managers should stay in the work (try features on staging as users would) and be more product-savvy. He expects organizational speed to be harder than automating code, tests and reviews. These are one company's self-reported numbers, and the post ends with a hiring pitch.
