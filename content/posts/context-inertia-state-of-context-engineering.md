---
title: "Context inertia: agents stay fixated until their context is cleared"
slug: "context-inertia-state-of-context-engineering"
date: 2026-10-25T23:06:00+0000
summary: "Drew Breunig's AGNTCon talk argues that every model capability creates a new failure mode. Long context brought poisoning and distraction; long-horizon agents bring 'fighting the weights' and 'context inertia', where an agent hyper-focuses on one idea and keeps pursuing it. Four recent papers and posts converge on the same fix: periodically clear the context while keeping the work in the environment. His practical advice is to reset around 200k tokens."
source: "https://www.dbreunig.com/2026/10/24/the-state-of-context-engineering.html"
source_title: "The State of Context Engineering"
source_author: "Drew Breunig"
source_site: "dbreunig.com"
source_date: "2026-10-24"
hn_url: "https://news.ycombinator.com/item?id=50235780"
---

**Bottom line:** Drew Breunig says context engineering is the job of deciding what should be deterministic code and what should be a probabilistic model call, and each new model capability creates a new failure mode to manage. For today's long-running agents, the one he highlights is "context inertia", and the fix is to delete the context.

**Long context (2024-25).** Million-token windows were pitched as "put everything in and let the model sort it out." That brought poisoning (a hallucinated fact, or Gemini 2.5 chasing a tea item from the wrong Pokémon game), distraction (a paper showed models follow contradictory examples in the history over explicit instructions as examples pile up), confusion, and clash. Gemini 2.5's own report noted the Pokémon agent began repeating past actions beyond roughly 100k tokens.

**The toolbox hasn't changed.** Six moves: augmentation (RAG, skills), tool loadout, quarantine (subagents), pruning, offloading to files or a database, and summarization (compaction). He shows how little code these take in a DSPy agent loop, about 16 lines of changes, and notes that an enterprise team loading 600 skills per engineer saw its agents degrade until loadout filtered them.

**Two new failure modes for agentic models.** "Fighting the weights" is when a model trained on particular harnesses resists your tools or instructions; heavy repetition or all-caps in a system prompt (Claude Code repeatedly demanding parallel tool calls) is a tell. "Context inertia" is when a model fixates on a hypothesis or subtask and keeps pursuing it. He cites Anthropic's self-replication harness (writes a short note every 90 minutes, then clears context), Microsoft Research's Fork-and-Flush, and Berkeley's "Scaling Discovery Requires Forgetting", where performance jumps after each context clear.

**Practical advice.** If a coding agent is behaving oddly at 800k tokens, reset it. His own warning point is about 200k tokens.

This is a lightly edited conference talk, so the claims about the cited papers are his summaries.
