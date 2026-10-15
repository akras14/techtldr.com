---
title: "Four ways to be more ambitious with an AI model: speed, altitude, depth, range"
slug: "ambitious-prompting-altitude-depth-range"
date: 2026-10-15T19:03:00+0000
summary: "Felix Rieseberg, who leads engineering for Claude.ai at Anthropic, argues most people under-use AI models and offers a framework of four kinds of ambition: speed, altitude (aim at the outcome, not the task), depth (work like a field-leading expert, on raw data, with outside validation) and range (borrow methods from other fields). He illustrates it with a quarterly-business-review deck and says the advice isn't Claude-specific."
source: "https://felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models/"
source_title: "How to be more ambitious: A tutorial for humans with models"
source_author: "Felix Rieseberg"
source_site: "felixrieseberg.com"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50100345"
---

**Bottom line:** Rieseberg says the gap between people getting remarkable results from models and those using them for small chores is largely about ambition. Speed (the same work faster) is already well understood, so he covers the other three.

## The techniques

- **Altitude:** ask for the goal beyond the task. Instead of fixing individual crashes, he asked Claude to keep the apps crash-free, which required giving it crash reports, a version-to-commit mapping and machines to reproduce bugs on. For a QBR deck, move from "update the numbers" to "what changed, why, and what should we do", then "which decisions are most urgent", then "design a better process than a quarterly deck".
- **Depth:** push the model to use what the best professionals use. Insist on databases and statistical models over markdown tables, set unreasonable goals (ask for 200 ms, not 5 s), have it adversarially check beliefs against raw data, and ask it to find blind spots. Don't have the model judge itself; give it an outside tool that returns feedback.
- **Range:** bring in other fields. Restate the task with verbs instead of nouns, and assemble a "crew" of specialists (analyst, editor, forensic accountant). Ask for what a short seller does, not for a role-play of one.
- **Requirement:** the model needs a computer, the ability to install software and access to relevant data (via MCP connectors or similar).

## Why he thinks it's needed

He ties it to self-efficacy: people given specific hard goals do better, and seeing others do something makes it easier to believe. He includes a reusable prompt that asks the model to say what a top professional would do, which raw material they'd start from, and what would independently verify the result before rewriting your prompt.
