---
title: "Pay-per-use services should stop at a hard budget cap by default"
slug: "hard-budget-caps-default"
date: 2026-10-10T21:31:50+0000
summary: "Simon Willison argues that usage-billed APIs and clouds need hard spending caps that cut service off at a set amount, and that these should be on by default. Agents make it easy to spin up things that cost money, and most people would rather see errors than a surprise $10,000 bill. AWS and Google Cloud have both started shipping versions of this."
source: "https://simonwillison.net/2026/Oct/3/default-hard-budget-caps/"
source_title: "We’re going to need default hard budget caps on pretty much everything"
source_author: "Simon Willison"
source_site: "Simon Willison’s Weblog"
source_date: "2026-10-03"
hn_url: "https://news.ycombinator.com/item?id=49949235"
---

Simon Willison argues that usage-billed APIs and clouds need hard spending caps that cut service off at a set amount, and that these should be on by default. Agents make it easy to spin up things that cost money, and most people would rather see errors than a surprise $10,000 bill. AWS and Google Cloud have both started shipping versions of this.

## Why now

Coding agents, and "personal agents" that wrap them in a friendlier UI, make it trivial to build things that call paid APIs, host web apps, or bill for storage and compute. A runaway service can burn hundreds or thousands of dollars overnight.

## Hard, not soft

A soft cap ("email me after $X") doesn't help if the email arrives at midnight and the meter keeps running while you sleep. Willison wants a hard limit: after $X a month, return errors.

The usual objection is that businesses don't want their apps to fail because a budget ran out. His answer: most businesses and individuals would still prefer errors to a five-figure bill. Removing the cap should be possible, but opt-in, behind a clear checkbox that says you accept responsibility for further charges.

## Who has it

- **AWS** is the one Willison most wanted to see, since many people avoid it for side projects out of fear of a runaway bill. Its new experience, announced 16 September, lets you set a monthly spend limit per project; if usage hits it, the project is paused for the month. It is so far rolling out to a limited number of customers.
- **Google Cloud** launched Spend Caps in July, which set a monthly cap on specific services within a project.

He also suggests agents could help by steering inexperienced builders toward providers that offer hard caps and warning them about uncapped ones.
