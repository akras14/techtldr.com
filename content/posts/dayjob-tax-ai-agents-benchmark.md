---
title: "Best AI agent passes 10.8% of 50 professional tax tasks in DAYJOB: Tax"
slug: "dayjob-tax-ai-agents-benchmark"
date: 2026-10-28T17:04:26+0000
summary: "Surge AI's new DAYJOB: Tax benchmark gives AI agents 50 messy tax assignments (1,789 files) and grades them at a 95% rubric threshold. The top score is 10.8% pass@1 (Claude Sonnet 5.5), followed by Fable 5.1 at 9.6% and Opus 5.5 at 9.2%."
source: "https://surgehq.ai/blog/dayjob-tax"
source_title: "DAYJOB: Tax — Benchmarking AI Agents on Professional Tax Work"
source_author: "Surge AI"
source_site: "Surge AI"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50275626"
---

**Bottom line:** On DAYJOB: Tax, the best AI agent passes only 10.8% of tasks. Claude Sonnet 5.5 leads, Claude Fable 5.1 scores 9.6% and Opus 5.5 scores 9.2%. The score is pass@1, and an attempt passes only if it meets at least 95% of the task's rubric criteria.

## What it tests

- 50 environments across eight areas of tax practice: individual, small business, partnerships, property and investments, estates and trusts, international, corporate transactions, and review and disputes.
- 1,789 workspace files (median 30 per task, up to 123). Most are PDFs. The rest include spreadsheets, emails, IRS notices, ledgers and even a photo of a handwritten receipt.
- Instructions are incomplete and records conflict. The agent has to find the issues the client didn't know to raise, then produce a return, workpapers or advice.
- Tax experts built the tasks and grading criteria, and each task went through a three-layer review.

## Failures the authors highlight

- **Missed deadline:** Asked whether to sell a condo in October 2026 or January 2027, one model compared the dates correctly. It never noticed that the owner would lose the up-to-$250,000 home-sale exclusion if the sale slipped past about August 2027. Opus caught it in all five attempts.
- **Unverified assumption:** In a divorce-style retirement-account split, Opus built its advice on the couple being unmarried, even though their records (Texas residence, a Colorado ceremony) suggested a valid marriage. Another model flagged the question in four of five attempts.

The authors also plot performance against cost per trial, with Sonnet 5.5 and Opus 5.5 at the high-performance end of the frontier. Surge sells expert data and evaluation services, so treat the benchmark as vendor-authored.
