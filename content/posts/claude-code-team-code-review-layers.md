---
title: "Claude Code's PRs doubled while reverts fell 40% with layered AI review"
slug: "claude-code-team-code-review-layers"
date: 2026-10-16T20:06:00+0000
summary: "The Claude Code team says merged PRs to its codebase more than doubled from June to September while the share reverted fell by over 40 percent. They credit moving most reading to machines: agent verification before a PR, AI and security review on every PR, risk-tiered human approval, and fast rollback. The goal is minimal human time without raising reverts or incidents."
source: "https://claude.dev/blog/solving-code-review-at-anthropic/"
source_title: "How the Claude Code team does code review"
source_author: "Addy Osmani, Morgan Westlee Lunt, Qing Wang"
source_site: "claude.dev"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50122951"
---

**Bottom line:** With Claude writing about 90% of Claude Code's own code, the team stopped treating "a human read every line" as the goal. They now optimize for scarce human attention, using layered automated checks and reserving people for risk and judgment calls. Revert and incident rates are the guardrail.

## The layers

1. **Verification before a PR.** Claude must run something (tests, a build, a screenshot) and show the output. A fresh-context subagent reviews the diff, and internal skills hunt bugs and simplify. Recurring complaints are fixed at the source in lint rules, `CLAUDE.md`, or a per-area `REVIEW.md`.
2. **AI review on every PR.** Several agents look for bugs, then others vote on each finding, and style-only findings are dropped. Findings are tagged Important, Nit or Pre-existing, and each must be fixed or closed with a reason. Substantive comments on internal PRs went from 16% to 54% in the research preview.
3. **Security review that blocks merging** while any finding is open.
4. **Ordinary blocking checks:** lint, types, cross-platform tests, build.
5. **Human approval scaled by risk.** Docs and flagged internal changes need none, ordinary changes one person, and the riskiest code two. A classifier picks the tier, an agent's approval never counts as a person's, and the person directing an agent counts as a reviewer.
6. **Merge queue, flags and gradual release.**
7. **Fast detection and rollback,** with flags that turn off without a deploy.

## Tuning the reviewer

- Past incidents and reverts form a daily eval set for the reviewer.
- Noise is measured by how people respond to comments.
- Agent author plus agent reviewer can loop: one PR went through 50+ rounds overnight, and about 59% of what the reviewer kept flagging was code present since its first pass. Fixes: a three-round cap, diff-only re-review, flagging the class of bug, and no repeated findings.

## Caveats

The authors say they don't yet have circuit breakers that tighten approvals when revert or incident numbers move, and engineers want them before more code moves to zero-human merges. Context sharing across the team is handled informally, with Claude-written explainers of code slices. The figures are the team's own, and the post is from Anthropic about its own product.
