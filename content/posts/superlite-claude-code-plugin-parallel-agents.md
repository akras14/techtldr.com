---
title: "Superlite plugin builds a 177-test project in 16 minutes, 52% faster than superpowers"
slug: "superlite-claude-code-plugin-parallel-agents"
date: 2026-10-19T05:02:50+0000
summary: "Superlite is a Claude Code plugin that takes one approval on a spec, then builds autonomously with parallel agents instead of stopping at human gates. In the author's own benchmark it finished a 177-test project in 16.1 minutes with zero human stops, versus a 33.6-minute mean for the superpowers workflow (52% less time). It is one author's limited comparison, which they call a strong hint rather than science."
source: "https://contextfirst.blog/superlite-hand-an-agent-a-spec/"
source_title: "Superlite: hand an agent a spec, come back to tested work"
source_author: "Iheatu Chris Wogu"
source_site: "contextfirst.blog"
source_date: "2026-10-18"
hn_url: "https://news.ycombinator.com/item?id=50147492"
---

**Bottom line:** Superlite is a Claude Code plugin for autonomous builds. After one approval it plans, runs independent tasks in parallel and reviews the result without further human stops. In the author's benchmark it built a 177-test project (static site generator plus spreadsheet engine) in 16.1 minutes with all hidden tests passing, versus a 33.6-minute mean for superpowers (52% less time) and 23.5 minutes for an orchestrated superpowers setup (31% less). The two plugins conflict and shouldn't be enabled together.

## Why superpowers was slow

The author profiled superpowers and found most time went to waiting:

- 8-12 human gates per feature (design, spec, plan and execution-method approvals).
- Strictly sequential execution, e.g. 21 dispatches in one queue.
- A review after every task plus re-review loops, about 12 of a 34-minute build.
- Full test suites re-run by several skills.
- Opus subagents on the critical path, at about half the speed of Sonnet.

## How superlite works

- **One approval.** Small fixes skip design; new projects get up to four questions and one design-and-plan document. After that it pauses only for destructive, outward-facing or security-sensitive actions such as pushing to main.
- **Parallel task graph.** Each task declares the files it owns, its interfaces and tests. Independent tasks run at once; the latest benchmark peaked at 18 agents.
- **Model tiers.** Opus coordinates but may not edit code; Sonnet builds most tasks and reviews; Haiku takes small, fully specified work; a fresh Opus does the final review from a log-based packet; fix-only agents handle findings.
- **Rules enforced in code.** Hooks stop the coordinator editing project code, push test runs to the background, block killing processes by name and require a lint pass before work starts.
- **Metrics.** Hooks log tokens, cost and time per agent, feeding predictions and a soft budget warning at 70% of the cap.

## Results

- Full project, latest run: 16.1 min, 177/177 hidden tests, 21 agents, 0 human stops, 30-second final review.
- Earlier three-run comparison: superlite mean 25.1 min (21.4-30.5) versus superpowers 33.6 min (21.2-47.6), all 177/177. Run-to-run spread was 9.1 vs 26.4 minutes.
- Smaller project (89 tests): 8.8 min and 10 dispatches versus 34.4 min and 21 sequential dispatches.

## Caveats

The harness builds the same project from the same spec with Opus and scores it with a hidden test suite, but it is the author's own limited benchmark of their own tool, on essentially two projects. Setup needs Claude Code, git, Python 3.10+ and bash; install via the marketplace entry `iha2/superlite`.
