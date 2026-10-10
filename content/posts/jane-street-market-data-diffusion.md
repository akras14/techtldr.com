---
title: "A Jane Street intern's autoregressive diffusion model of order-book data"
slug: "jane-street-market-data-diffusion"
date: 2026-10-10T03:02:38+0000
summary: "A Jane Street intern built an event-level autoregressive diffusion model of US equities order-book data. Fully continuous diffusion failed because market data is spiky and partly discrete; flow matching plus a technique called atom smoothing worked reasonably well, though the model isn't yet a realistic generator."
source: "https://blog.janestreet.com/can-you-use-autoregressive-diffusion-to-generate-market-data/"
source_title: "Can you use autoregressive diffusion to generate market data?"
source_author: "James Somers"
source_site: "Jane Street Blog"
source_date: "2026-09-30"
hn_url: "https://news.ycombinator.com/item?id=50021410"
---
Summer intern Kavish trained a model on four years of US equities events (trades, order book best-bid/offer changes and so on) to generate the next event, appended autoregressively. The design is a causally masked transformer encoder, a small head predicting event kind, and a diffusion head for continuous targets like price and elapsed time.

Key findings:
- DDPM diverged: 88–95% of values landed over 8 standard deviations from the mean. Flow matching worked well out of the box.
- The core problem is that market data is neither continuous nor discrete. Timing has point masses at zero, at the earliest possible reaction time and at whole seconds, and prices cluster near the current bid and ask.
- Fix 1: split the categorical head into 20 classes so atoms like zero time gaps are handled categorically. It improved marginals but required heavy hand-engineering that won't scale to more features.
- Fix 2, "atom smoothing": smooth the spiky distribution, then re-sculpt the spike without creating a discontinuity. This depends on choosing the right variable representation.

Smoothing produced much more realistic one-step samples, judged by total variation divergence and a real-vs-generated classifier. Rollouts degrade further out, so the model isn't a realistic generator yet, but the project clarified what matters for building one.
