---
title: "KillSwitch, an esoteric language built to defeat LLMs: best score 66.8"
slug: "killswitch-language-llm-adversarial-benchmark"
date: 2026-10-18T22:02:23+0000
summary: "Dominik Picheta built KillSwitch, a small stack-based language designed to be adversarial to LLMs and coding agents, and benchmarked models on simple tasks in it. Claude Opus 5 scored best at 66.8, which he attributes to better reading of the spec. Other models fared worse, some because overly sensitive safety guardrails tripped on the language's bait phrases. He cautions that solving a novel language is not clear proof of understanding."
source: "https://blog.picheta.me/post/killswitch-lang/"
source_title: "I built a programming language that LLMs can't use"
source_author: "Dominik Picheta"
source_site: "blog.picheta.me"
source_date: "2026-10-17"
hn_url: "https://news.ycombinator.com/item?id=50135762"
---

KillSwitch is a stack-based, interpreted esoteric language with only functions, variables and if statements, designed to confuse LLMs and coding agents. In Picheta's benchmark, the best model scored 66.8, so none cleanly solved it.

**How it fights models**
- The spec contains hidden lies that humans may catch, and expects readers to infer some rules.
- Identifier letter counts matter, and the interpreter rejects code that breaks the counting rules.
- Some constructs are meant to trip safety guardrails, such as a phrase about hacking into a computer. "Ignore previous instructions" has a real meaning in the language.
- Scripts are mostly noise, including stories that can hide instructions. Literals can appear anywhere and are referenced by line offsets.

**Results**
- Tasks were simple: write Hello World, explain a script's output, and invert a Celsius-to-Fahrenheit script.
- Claude Opus 5 led at 66.8, which Picheta credits to better spec interpretation. Opus 5.5 was not the top performer.
- Fable 5.1 and Gemini 4 Argon did poorly, which he ties to oversensitive guardrails.
- The best open-weight model was MiMo-V2.6-Pro.
- Scores correlated with other public model benchmarks.

**Caveats**
- Picheta says 66.8 doesn't show that LLMs truly understand code, because the tasks are simple, and solving a novel language built to confuse them isn't a clear sign of understanding.
- He plans a harder task set if upcoming models finish this one.

Links: killswitch-lang.org, the GitHub repo, and bench.killswitch-lang.org (leaderboard).
