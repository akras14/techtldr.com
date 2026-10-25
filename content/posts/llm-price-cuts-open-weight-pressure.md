---
title: "LLM prices fell up to 80% since June, and open-weight rivals are why"
slug: "llm-price-cuts-open-weight-pressure"
date: 2026-10-25T23:05:00+0000
summary: "Max Woolf lays out a four-month timeline of OpenAI and Anthropic price cuts, up to 80% on small models and 40-75% on flagships. He argues the cause is commodity competition: swapping models is trivial and 29 providers now host DeepSeek V4.1 Flash on OpenRouter. He doubts the 'prices will spike once the subsidy ends' theory, though he expects subscription allowances to be cut before API prices."
source: "https://minimaxir.com/2026/10/cheap-llms/"
source_title: "OpenAI and Anthropic cut their LLM prices by up to 80% within a few months. Why?"
source_author: "Max Woolf"
source_site: "Max Woolf's Blog"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50236102"
---

**Bottom line:** Max Woolf argues that the steep LLM price cuts since June come from commodity pricing pressure, mainly open-weight models served by many competing providers, and that the "prices will jump once the subsidy ends" story doesn't fit the evidence.

**The timeline.** GPT-5.6 Luna was cut 80% in July (to $0.20 / $1.20 per million input/output tokens). GPT-5.6 Sol got a temporary ~20% cut in August. Anthropic made Sonnet 5's launch price permanent and, with Fable 5.1, cut cache-read cost by 75%. In September, OpenAI's GPT-6 Sol and Luna launched at half the old prices, and Opus 5.5 came in 40-60% cheaper than Opus 5. GPT-6.1 Sol then halved its cache price, making that family about 75% cheaper overall. Woolf notes that cache reads are 96-99% of token spend in coding agents, so cache pricing drives real cost.

**Why, per Woolf.** Since agentic coding took off, models are easy to swap (a harness toggle or an OpenRouter string), so there is little moat. DeepSeek V4 Flash's tiny cache price and a growing field of hosts (13 providers in May, 29 now for the 1017 variant) set a floor. Moonshot's Kimi K3.1 Pro is priced at half Opus 5.5's input/output rates for similar benchmark scores. He thinks the token price will settle near marginal cost, and that the Uber-style loss-leader analogy fails because OpenAI and Anthropic aren't a duopoly with high entry barriers.

**Caveats he raises.** Both labs are rumored to be nearing IPOs (Anthropic within months), so the cuts look odd given their losses; inference margins may still be healthy. Subscription value is more at risk than API prices: OpenAI cut the $200/month Codex allowance by 50% alongside GPT-6.1 Sol. Aggregate spend keeps rising because models improve faster than they get cheaper. He also suggests that more efficient models could reduce the need for data-center buildouts, though he doubts that happens.

This is one writer's analysis, and the IPO and margin points are speculation.
