---
title: "AI-assisted teams saw 54% more bugs per developer; a layered verification pipeline"
slug: "ai-code-verification-pipeline"
date: 2026-10-17T21:02:25+0000
summary: "Human review can't keep pace with AI-written code, so Mihai Avram argues teams should keep the speed and add automated verification that builds justified confidence. He cites a 2026 Faros study of 22,000 developers (correlational) where higher AI adoption went with 54% more bugs per developer, about 5x longer median PR review time and over 3x the incidents per PR. His answer is a staged pipeline of cheap checks first and agentic review at the PR, with a human still accountable for the merge."
source: "https://mihaisplace.com/blog/ai-can-write-the-code-who-verifies-it"
source_title: "AI Can Write the Code. Who Verifies It?"
source_author: "Mihai Avram"
source_site: "mihaisplace.com"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50133713"
---

**Bottom line:** if AI makes writing code cheap, verification has to become a first-class part of the process. No system guarantees bug-free software; the goal is to catch more problems before users do, and "automated review is not the same as a safe merge decision."

## The numbers

A 2026 Faros study (two years of telemetry, 22,000 developers, 4,000 teams) compared lower and higher AI adoption:

- 54% more bugs per developer
- About 5x median PR review time
- More than 3x incidents per PR

Avram flags these as correlations, not proof of causation. A later Faros Q3 report found some effects easing while downstream verification and remediation costs kept piling up.

## Staged pipeline

Checks are layered by cost and risk:

- **Pre-commit:** formatting, linting, unit tests, secret scanning
- **Post-push:** broader tests, SAST, dependency scans
- **Pull request:** agentic bug, security and spec-compliance review, plus required checks
- **Pre-production:** E2E tests, migration validation, smoke tests, canary rollouts for risky changes
- **Production:** monitoring, anomaly detection, rollback

## Tools surveyed

Superpowers (skills that push agents to plan, TDD and self-review), RoboRev (local daemon reviewing commits), Claude Code Review, OpenAI Codex Review, GitHub Copilot Code Review and Cursor Bugbot. His advice: review with a different model than the one that wrote the code, pair it with deterministic checks, and keep a human accountable. His sample GitHub Actions Codex job is read-only and needs hardening (pinned SHAs, restricted permissions, keeping API keys away from untrusted contributors).

## His workflow

Iterate on a spec, break it into commit-sized steps with planned tests, have a second model review the plan and each commit, review the whole change against the spec, then human review of the full diff at PR time and smoke tests before and after release.
