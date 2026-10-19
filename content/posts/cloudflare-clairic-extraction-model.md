---
title: "Cloudflare's Clairic extracts typed values in one pass, with no output tokens"
slug: "cloudflare-clairic-extraction-model"
date: 2026-10-19T21:10:00+0000
summary: "Cloudflare released Clairic in preview on Workers AI (@cf/cloudflare/clairic), a \"System One\" model that answers in a single forward pass and generates no tokens. You give it text (and up to four images) plus typed fields, and it returns extracted, computed, estimated or forecast values, each with a confidence score. Open weights are promised soon."
source: "https://blog.cloudflare.com/clairic-extraction-model/"
source_title: "Introducing Clairic, a new System One class of extraction models"
source_author: "Alex Reneau, Michelle Chen, Kevin Flansburg"
source_site: "Cloudflare Blog"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50155907"
---
Clairic is pitched as a replacement for LLM calls that just pull structured values out of text: invoice totals, delivery dates, days late, repair cost, tomorrow's traffic. Because it does one parallel pass and emits no tokens, it bills only for input, typically responds in under a second, and can't return malformed JSON. Cloudflare says it is more accurate, faster and cheaper than LLMs for these tasks, but the post gives no head-to-head benchmark.

A request is a text state plus a map of named, typed fields (text, number, date, time, datetime, duration, list, estimate, forecast). Text answers come back with character offsets so you can trace them to the source. Every scalar answer carries a confidence and `p_null`, the probability the value isn't in the input, and a missing value returns null instead of a plausible guess. Computed fields work too: in the demo it sums price and tax and counts days between two dates. Estimate and forecast fields return a median and an 80% interval inside bounds you declare. There is no field limit; extra fields are split into parallel passes.

The one quantified test: forecasting daily TSA checkpoint counts for 21 days, it was off by 3.3% on average, and 17 of 21 actuals landed inside the 80% interval. Three of the four misses fell on the Columbus Day weekend. Cloudflare says it isn't a substitute for a dedicated time-series model.

Clairic follows decision-style System One models (Jev from TypeSafe, and Clef) that pick among listed options with probabilities. Extraction models cover the "number or typed value" case, with LLMs left for free-form text.
