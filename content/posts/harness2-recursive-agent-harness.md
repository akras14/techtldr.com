---
title: "Harness²: Google Research tool that refines an agent's harness per task, up to +11.8 points"
slug: "harness2-recursive-agent-harness"
date: 2026-10-19T14:02:47+0000
summary: "Google Research released Harness², an Apache-2.0 framework that improves the harness around a fixed model (prompts, memory, tools, skills, subagents) instead of its weights. For each task it proposes edits, probes them, and composes a refined harness; a persistent improvement harness carries lessons to later tasks. One step lifted GLM-5 and Gemini 3.5 Flash by up to 11.5 points on WorkBuddy-Bench, 4.5 on JobBench and 11.8 points of partial pass rate on LAB."
source: "https://github.com/google-research/harness2"
source_title: "Harness2: Recursive Agent Harnessing for an Open World"
source_author: "Google Research"
source_site: "GitHub"
hn_url: "https://news.ycombinator.com/item?id=50151059"
---

Harness² treats the "harness" (everything around the model that lets it act as an agent) as the thing to optimize, with model weights held fixed. It exposes eight editable components: system prompt, guardrails, memory, tool guidance, subagents, skills, scripts and plugins.

## Two levels of recursion

- **Task-level:** a Propose–Probe–Compose loop. An editor proposes harness edits, candidates are run on the same task to probe their effects, and a composer merges a refined harness from the behavioral differences between runs. It needs no ground-truth feedback.
- **Harnessing-level:** a persistent improvement harness is updated with adopted harnesses and evidence-backed lessons after each task, guiding refinement on future, unseen tasks.

## Results and use

Across three professional benchmarks and four model–harness setups, a single step improved GLM-5 and Gemini 3.5 Flash over their base harnesses: up to 11.5 points on WorkBuddy-Bench, 4.5 on JobBench and 11.8 percentage points of partial pass rate on LAB. More task-level recursion budget gave further gains. These are the authors' own numbers.

The release supports parallel and sequential recursion, OpenCode and Codex executors, and those three benchmarks. It needs Linux, Python 3.12 and uv. A credential-free local demo (`harness2-demo`) runs with deterministic editors; model-backed benchmark runs use Vertex AI and incur API costs.
