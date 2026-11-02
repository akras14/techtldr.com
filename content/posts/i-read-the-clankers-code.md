---
title: "Why one engineer still reads every line of AI-written code"
slug: "i-read-the-clankers-code"
date: 2026-11-02T20:03:00+0000
summary: "Charles Féval argues that if your name is on the PR and you're on call, you own AI-generated code and must read it. Agents are fast and often good, but they work by addition, don't learn from correction, and no repeatable control mechanism yet replaces review."
source: "https://fev.al/posts/i-read-the-code/"
source_title: "I Read The Clankers' Code"
source_author: "Charles Féval"
source_site: "fev.al"
source_date: "2026-10-24"
hn_url: "https://news.ycombinator.com/item?id=50340362"
---

**Bottom line:** The author doesn't say "don't use coding agents." He says that as long as your name is on the PR and your pager is in the rotation, you own what the agent wrote, so you have to review it. Throwaway toys for a sole user are the exception.

- **Failure modes he lists:** code built only by addition, 100% coverage of the wrong conditions, functions reimplemented many times, useless comments and wrong docs, removed "don't touch this" code, careless dependencies, invented features, tests altered to pass, and confident but nonsensical root-cause analyses.
- **It compounds:** the larger the codebase, the likelier garbage gets in, and later work builds on it. Poor code quality also makes human review impossible.
- **It doesn't learn:** unlike a junior developer, agents ignore repeated instructions as context grows, and system prompts, `CLAUDE.md`-style files and reviews by other models haven't reliably fixed this in his experience.
- **What he'd need:** formalized processes and proven, repeatable controls on output quality, which he says don't exist yet. "Secret herbs and spices" prompting doesn't count as engineering.

It's an opinion piece from a 20-year developer, not a measurement. He concedes the agent's code is often better than his own after review.
