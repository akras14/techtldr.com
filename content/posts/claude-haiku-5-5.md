---
title: "Claude Haiku 5.5: Anthropic's Small Model Gets Much Smarter and About 75% Cheaper"
slug: "claude-haiku-5-5"
date: 2026-10-07T23:55:00-07:00
summary: "Anthropic released Claude Haiku 5.5, a fast, cheap model for high-volume work like summaries, classification, subagents, and browser use. It costs about 75% less to run than Haiku 4.5 and scores far higher on benchmarks. Anthropic also halved Sonnet 5.5's cache-read price and added monthly API credits for Max and Team subscribers."
source: "https://www.anthropic.com/claude-haiku-5-5"
source_title: "Introducing Claude Haiku 5.5"
source_author: "Anthropic"
source_site: "Anthropic"
---

Anthropic released Claude Haiku 5.5, a fast, cheap model for high-volume work like summaries, classification, subagents, and browser use. It costs about 75% less to run than Haiku 4.5 and scores far higher on benchmarks. Anthropic also halved Sonnet 5.5's cache-read price and added monthly API credits for Max and Team subscribers.

## What it's for

- **High-volume, cost-sensitive tasks:** summaries, context compaction, database queries, classification.
- **Subagent work** alongside Opus 5.5 or Sonnet 5.5 on coding tasks.
- **Latency-sensitive tasks** such as live customer support and browser use. It's Anthropic's fastest model at standard speed, though Opus in Fast Mode is quicker.
- **Not complex agentic coding.** Anthropic still recommends Sonnet 5.5 or Opus 5.5 for that.

It's also the **first Haiku with an adjustable effort setting**, so you can trade cost for intelligence per request.

## Benchmarks (Haiku 5.5 vs Haiku 4.5)

| Benchmark | Haiku 5.5 | Haiku 4.5 | Sonnet 5.5 (reference) |
|---|---|---|---|
| OSWorld 2.1 (computer use) | 72.4% | 15.7% | 83.9% |
| Terminal-Bench 4.0 (agentic coding) | 39.2% | 0.0% | 70.6% |
| Humanity's Last Exam, no tools | 45.9% | 10.2% | 56.9% |
| GDPval-AA v2.1 (knowledge work, Elo) | 1620 | 735 | 1840 |
| Chartography (visual reasoning) | 46.4% | 6.4% | 61.6% |

It also beats OpenAI's GPT-6 Luna on every benchmark where Anthropic shows both.

## Pricing

Prices are per million tokens, for prompts up to 100k tokens / over 100k tokens:

- **Input:** $0.10 / $0.50 (Haiku 4.5: $1.00)
- **Output:** $0.50 / $2.50 (Haiku 4.5: $5.00)
- **Cache reads:** $0.01 / $0.05

That's **90% cheaper than Haiku 4.5 for prompts under 100k tokens** (about 90% of past Haiku traffic) and 50% cheaper above that. A new tokenizer uses slightly more tokens per task, which Anthropic says brings the overall saving to **about 75%**.

## What early customers reported

- **Asana:** over 30% lower task latency and up to 2.5× faster per agent turn.
- **HubSpot:** 92.8% on its CRM eval suite, the best small-model score it has seen.
- **AlphaSense:** a significant gain over Haiku 4.5 (0.84 vs 0.76) on a feature that runs about 8M calls a week.
- **Box:** 11 points better than Haiku 4.5 at about half the latency.
- **Cognition:** used as the "sidekick" model in Devin, it kept a top-tier FrontierCode score while cutting cost and latency.

## Safety

Anthropic reports large improvements over Haiku 4.5 on alignment evaluations. Its cybersecurity safeguards allow more defensive work than Sonnet 5.5's but still block penetration testing. Its biology safeguards match the other 5.x models. Verification programs exist for organizations that need broader cyber or life-sciences access.

## Other announcements

- **Sonnet 5.5 cache reads are 50% cheaper** ($0.20 → $0.10 per million tokens), which makes most agentic workloads about 20% cheaper.
- **Monthly API credits for subscribers:** $100 (Max 5x), $200 (Max 20x), and up to $500 pooled for Team, usable on any model.
- **SDK support** for computer use and browser use (beta) in Python and TypeScript.

Haiku 5.5 is available now on the Claude Platform (`claude-haiku-5-5`), AWS, Google Cloud, and Azure.
