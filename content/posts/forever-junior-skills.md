---
title: "A Criteo junior's four habits for growing to senior while agents code"
slug: "forever-junior-skills"
date: 2026-10-10T21:31:50+0000
summary: "A junior engineer at Criteo worries that if agents do the work, juniors never become seniors. The proposed answer is four habits: question the agent until you understand its work, learn review by emulating what seniors flag, occasionally make changes by hand, and ask real seniors instead of the agent. The author says the approach led to a promotion nomination ahead of schedule."
source: "https://tech.criteo.com/blog/human-skills-ai-cant-develop-junior-engineers/"
source_title: "Forever Junior: The Skills AI Can’t Develop For You"
source_author: "Elise Baturone"
source_site: "Criteo Tech Community"
source_date: "2026-10-07"
hn_url: "https://news.ycombinator.com/item?id=49989684"
---

A junior engineer at Criteo worries that if agents do the work, juniors never become seniors. The proposed answer is four habits: question the agent until you understand its work, learn review by emulating what seniors flag, occasionally make changes by hand, and ask real seniors instead of the agent. The author says the approach led to a promotion nomination ahead of schedule.

## The fear

School taught that you get better by coding; now engineers are told to delegate to agents. Elise Baturone's worry isn't engineers disappearing but seniors disappearing, leaving "forever juniors" who only review one agent's work with another.

From watching teammates, Baturone defines seniors as people who can defend shipped code, argue design choices, tell when an agent is going wrong, know when to delegate, and teach others. Baturone started with a skill.md to make the agent behave like a mentor, then realized the real gap was in personal habits, not the agent's.

## 1. Curiosity

Baturone had the agent run a quiz after each change (explain it, predict what breaks, defend a trade-off). The questions were relevant only about half the time, but they forced a close read. Asking an agent to justify a dubious suggestion often surfaces the flaw. It costs tokens, but code that's actually understood fails less often in production.

## 2. Emulation

Past review comments are a training set. Baturone's team, which owns a product catalog, kept pointing out that new entity types resembled existing ones. So the approach became: use an existing entity as a template, justify every deviation, and write that pattern into the skill file so the agent's drafts match team conventions.

## 3. Autonomy

The skill blocks the agent from editing files, running migrations or committing until a plan is approved. Occasionally Baturone uses that pause to make a small change by hand. That builds:

- **Architecture sense:** feeling where an agent-built design is wrong.
- **Algorithmic understanding:** knowing the how, not just the what.
- **Token efficiency:** knowing where things live, and when an IDE rename beats an agent.
- **Confidence** in being able to do the job without an agent.

## 4. Collaboration

The skill drafts Slack messages to seniors for judgment calls, but copying the draft slid into copying answers. The fix: write your own questions, optionally have AI check your understanding first, and accept being corrected.

The skill file now gets opened about once a day instead of every session. The post asks seniors to share how they're adapting mentoring, and argues room to learn has to be taken by juniors, given by mentors, and allowed by leadership.
