---
title: "Opus 5.5 one-shot a three.js Invisible Cities in 85 minutes for $74"
slug: "opus-invisible-cities"
date: 2026-10-10T21:31:50+0000
summary: "Piotr Migdał gave GPT-6 Astra and Claude Opus 5.5 the same one-shot prompt to build a three.js visualization of Italo Calvino's Invisible Cities. Both produced working results; Astra's took 53 minutes and about $10 but had design slop, while Opus 5.5 took 1 hour 25 minutes with six parallel subagents and about $74, and left the author \"mesmerized.\""
source: "https://quesma.com/blog/invisible-cities-one-shot/"
source_title: "I gave Opus 5.5 one prompt and six hours to visualize Invisible Cities"
source_author: "Piotr Migdał"
source_site: "Quesma Blog"
source_date: "2026-10-07"
hn_url: "https://news.ycombinator.com/item?id=50004790"
---

Piotr Migdał gave GPT-6 Astra and Claude Opus 5.5 the same one-shot prompt to build a three.js visualization of Italo Calvino's Invisible Cities. Both produced working results; Astra's took 53 minutes and about $10 but had design slop, while Opus 5.5 took 1 hour 25 minutes with six parallel subagents and about $74, and left the author "mesmerized."

## Background

Migdał builds data visualizations and explorable explanations. Earlier models helped but needed lots of cleanup: Opus 4.8 drafted well but left AI slop and overlapping visuals, and Fable 5.1 handled data well but needed design hand-holding. A post showing Opus 5.5 build an interactive camera-lens lab in one shot ($25.66, 1h26m) prompted this test.

## The prompt

> Make a three.js (pnpm) visualization of all Invisible Cities by Italo Calvino. Don't ask questions, it is a one-shot task. You have 6h of work, use it until it becomes a masterpiece.

The book describes 55 imagined cities, each an emotion or state of mind.

## Results

- **GPT-6 Astra (Codex, medium effort):** worked end to end in 53 minutes for about $10. Weaknesses: many unnecessary concepts and comments adding visual noise, and obvious captions. Oddly, it adopted a "Claude" visual style: beige background, numbers like "05".
- **Claude Opus 5.5 (Claude Code):** claimed to use "roughly half" the six hours but actually took 1 hour 25 minutes, with six parallel subagents adding up to about seven agent-hours, for about $74. Migdał wanted to scold it for stopping early, then saw the result: "And I'm mesmerized!"

Both interactive results and their code are linked from the post. The post invites others to try the prompt with other models and harnesses, and ends by asking what the author's own place in making interactive media will be.
