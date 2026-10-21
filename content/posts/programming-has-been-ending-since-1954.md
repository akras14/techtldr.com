---
title: "Programming has been 'ending' since FORTRAN in 1954"
slug: "programming-has-been-ending-since-1954"
date: 2026-10-21T19:06:00+0000
summary: "Every layer of automation, from FORTRAN to Copilot, was pitched as the end of coding, and each time the job moved up a level while the field grew. The author argues AI agents are the same pattern at much higher speed. Survey and telemetry data show the bottleneck shifting to review and to deciding what to build, and entry-level hiring is where the pain shows up first."
source: "https://hrih.si/articles/the-compiler-was-supposed-to-end-programming/"
source_title: "The compiler was supposed to end programming too"
source_site: "hrih.si"
hn_url: "https://news.ycombinator.com/item?id=50185045"
---

IBM's 1954 FORTRAN report said the language "should virtually eliminate coding and debugging." It eliminated one kind of coding, and programmers moved up to bigger problems. The author lists the same pattern at each rung: assemblers, C, a 1981 BASIC generator called The Last One, Stack Overflow, npm, Copilot and now coding agents.

What differs this time is speed. Copilot's preview shipped in 2021, and by 2025 84% of developers in Stack Overflow's survey used or planned to use AI tools. Sentiment has not followed: favorable views fell from 77% in 2023 to 62% in 2026, and "almost right, but not quite" was the top frustration.

- **Feeling fast isn't measuring fast.** In METR's 2025 trial, 16 experienced developers took 19% longer with AI while believing they were 20% faster. A larger follow-up was abandoned as unreliable, partly because developers refused to work without AI.
- **Agents are getting longer-running.** METR's time horizon has doubled roughly every six months (every four since 2023), reaching about twelve hours for Claude Opus 4.6. The author extrapolates cautiously to a 2031 where agents work overnight and humans review and decide.
- **The queue moves to review.** Faros AI telemetry from 22,000 developers shows 34% more tasks per developer, five times the median review time, 31% more PRs merged with no review, and incidents per PR more than tripled.
- **Quality and junior hiring.** Veracode found security flaws in 45% of model outputs, and Stanford payroll data puts employment for developers aged 22 to 25 about 20% below its late-2022 peak, with no comparable gap for older ones. The authors call it descriptive, not causal.

The author's conclusion, citing Brooks' "No Silver Bullet": AI removes accidental difficulty, but working out what the software should do remains the human job.
