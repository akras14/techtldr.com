---
title: "A month of DeepSeek 4.1 Flash: Opus-like work, rarely over $1 a session"
slug: "deepseek-4-1-flash-cheap"
date: 2026-10-10T21:31:50+0000
summary: "After a month of heavy use across a dozen projects, the author says DeepSeek 4.1 Flash is hard to tell apart from Opus in day-to-day coding, yet all-day sessions rarely cost more than $1. That gap is why they think frontier labs should be worried: Chinese labs, skipping much of the training cost, can sell \"close enough\" at a fraction of the price."
source: "https://www.dgt.is/blog/2026-10-07-deepseek-freek-out/"
source_title: "Why Isn't The Industry Freaking Out About DeepSeek 4.1 Flash?"
source_site: "dgt.is"
source_date: "2026-10-07"
hn_url: "https://news.ycombinator.com/item?id=50000488"
---

After a month of heavy use across a dozen projects, the author says DeepSeek 4.1 Flash is hard to tell apart from Opus in day-to-day coding, yet all-day sessions rarely cost more than $1. That gap is why they think frontier labs should be worried: Chinese labs, skipping much of the training cost, can sell "close enough" at a fraction of the price.

This is a subjective account, not a benchmark study; the author links to other benchmarks for that.

## Good enough changes how you work

- **Cost.** On a $10/month OpenCode Go subscription, DeepSeek is effectively unlimited for them. Expected cost rarely exceeds $1 a session, even for sessions that run most of a day. A trivial task costs about $0.003 instead of $1.
- **Habits.** Cheap tokens make it fine to throw mindless tasks or exploratory UI "monkey testing" at the model.
- **Where frontier models still come in.** They use Flash for planning and research too, and bring in Opus 5.5 for a final review on occasional critical tasks, which catches a few edge cases. DeepSeek then makes the fixes. The point is fresh eyes more than more capability.

## Why it's cheap

Per the author, DeepSeek shrank the KV cache roughly 437x compared to its V1 model. Holding that cache in GPU memory is a major cost of long coding sessions. The author suggests this also means lower energy use.

## The argument

- Frontier labs are maybe a month or two ahead, but distilled Chinese models handle the same workloads. The author notes the training-data dispute ("they stole Claude's training") but sets it aside, saying most developers just want value.
- Their analogy: big pharma vs. generic manufacturers that skip R&D. Not identical copies, but close enough, at about a 90% discount.
- For self-hosters, they say the economics don't favor running it yourself today unless privacy is the goal.

In a postscript, the author says the post "hit a nerve" on Hacker News.
