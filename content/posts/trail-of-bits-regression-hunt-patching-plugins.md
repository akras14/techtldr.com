---
title: "Trail of Bits: 21 regressions in 130+ merged AI-assisted patches"
slug: "trail-of-bits-regression-hunt-patching-plugins"
date: 2026-10-21T18:02:19+0000
summary: "Trail of Bits audited its own Patch the Planet patches (done with OpenAI) and found 21 confirmed regressions among 130+ merged PRs: 16 functional, 1 performance, 4 build/test, none breaking a security protection. In response it released two agent plugins, patching-bugs and regression-hunt, which standardize how patches are written and checked. In preliminary evaluation, regression-hunt produced 25% fewer false-positive findings than a baseline Codex agent asked to review the PR for regressions."
source: "https://blog.trailofbits.com/2026/10/21/your-ai-agent-fixed-the-bug-but-what-else-did-it-change/"
source_title: "Your AI agent fixed the bug, but what else did it change?"
source_author: "Anish Naik, Marcelo Morales"
source_site: "Trail of Bits Blog"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50183253"
---

Trail of Bits reviewed every patch its engineers authored and maintainers merged under Patch the Planet, its joint initiative with OpenAI, and confirmed 21 regressions across more than 130 PRs: 16 functional, one performance and four build/test. None broke a security protection. To reduce both patch-introduced regressions and review noise, it released two plugins for coding agents, `patching-bugs` and `regression-hunt`.

**The audit.** An agent hunted for regressions in each patch and confirmed them with a proof-of-concept run before and after the change; engineers then reviewed the results. A single run produced 58 candidates, of which 21 were real (about 36% precision), so the review was noisy. The authors concluded they needed a standard way to write patches and a standard, written definition of a regression.

**regression-hunt** traces the diff through its callers to real interfaces, reproduces each candidate on a live deployment, replays the proof with a fresh-context agent, and has an adjudication step that rejects false positives, incomplete fixes, latent bugs and pre-existing bugs. It can feed a threat model to every step. Against the 21 known regressions, it produced 25% fewer false positives than a baseline Codex agent prompted to "review the PR for regressions" (95% CI 14% to 38%). The stricter evidence bar has a cost: some known regressions were left as "borderline" rather than confirmed.

**patching-bugs** distills practices from thousands of Codex sessions by their engineers. Its "three-arm proof" runs the same reproduction against the original code (must fail), the patched code (must pass) and a full revert (must fail again). It also aims to repair the violated security invariant rather than the single bug, and it always runs regression-hunt.

Both plugins use strict input/output schemas and require a live proof of concept for every claim; a step that can't produce one is marked inconclusive. Next steps are an evaluation of patching-bugs and analysis of agent traces.
