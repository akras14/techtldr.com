---
title: "Agentic coding as clay: working as an artisan or a factory"
slug: "code-is-clay-agentic-coding"
date: 2026-10-20T03:02:35+0000
summary: "Alberto Denia argues that agentic coding has made code malleable: changes are cheap enough to model, reshape or discard, so judgment (taste, architecture, guardrails) matters more than ever. He sees two workflow extremes, the artisan who stays in the loop and judges every iteration, and the factory, a pipeline of specs, generators and verifiers that escalates to a human only as a last resort. Most real workflows will mix both."
source: "https://www.albertodenia.com/writing/code-is-clay-now"
source_title: "Code is clay now"
source_author: "Alberto Denia"
source_site: "albertodenia.com"
source_date: "2026-09-25"
hn_url: "https://news.ycombinator.com/item?id=50153531"
---

**Bottom line:** With agentic coding the cost of changing code has collapsed, so code now behaves like clay rather than stone. You can sketch a rough shape, see how it feels and reshape or restart. Human judgment is still what keeps the result from collapsing into a puddle; the open question is when to apply it.

## Two ways to work with it

- **The artisan** stays in the loop. Fast feedback is the appeal: think of a feature, plan it with the agent, and review it within a coffee break. The author built a calc block for his Markdown editor in about half a soda. Staying in the loop and judging the code's shape, not just the user-facing surface, is what separates this from vibe coding.
- **The factory** removes the human from the loop. It is a pipeline of specs, generators and verifiers (tests, evals, guardrails, reviewing agents) that escalates to a person only as a last resort. It works best in well-structured codebases with repeatable patterns and clear criteria, such as adding the eleventh CRUD endpoint or grinding through many minor bugs.

## Judgment moves, it doesn't vanish

The artisan applies judgment at every iteration; the factory applies it upfront, when building the pipeline. The author suspects that setting up factories that produce reliable outcomes will become a large part of engineering. He expects the two approaches to be complementary: large products need artisanship for novel or feel-sensitive work, and solo developers will run factories for boring, repeatable parts.
