---
title: "Agents built the same app in 12 JS stacks: $2.67 to $4.97 per run"
slug: "ai-cost-of-js-frameworks"
date: 2026-11-01T09:05:00+0000
summary: "Nolan Lawson had Claude (Opus 5.5, high effort) build the same small Pokémon app in 12 JavaScript stacks, 3 to 5 runs each, and measured cost, time, tokens and Lighthouse results. Hono+htmx ($2.67), Astro ($2.76) and Nuxt ($2.82) were cheapest by median; SvelteKit ($4.28), Gracile+Lit ($4.65) and Angular ($4.97) cost the most. Angular failed to produce a working app in two runs. Plain Vite with no framework ($2.98) beat several frameworks. Lighthouse performance and accessibility scores were nearly saturated everywhere, so JS shipped is the main differentiator: Astro 0 KiB, Next.js 142 KiB. Run-to-run variance is large and the app is a small content site, so treat it as one data point, not a verdict."
source: "https://nolanlawson.com/2026/10/28/the-ai-cost-of-javascript-frameworks/"
source_title: "The AI cost of JavaScript frameworks"
source_author: "Nolan Lawson"
source_site: "nolanlawson.com"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50323088"
---

**Bottom line:** the framework you pick changes what an agent costs to build the app, but not by an order of magnitude, and noise between runs is about as big as the gaps.

## Setup

- One small, content-heavy Pokémon app (PokéAPI): list, search, filter. Closer to Wikipedia than to Figma.
- Opus 5.5 at high effort, latest version of each framework, official MCP or skills where they exist. No Tailwind or component kits. Agents got a spec conformance suite (tested against the accessibility tree) but weren't told about Lighthouse.
- Median of 5 runs per stack (3 for Angular, which failed to produce a working app twice).

## Results (medians)

| Stack | Cost | Time | Tokens | JS shipped |
|---|---|---|---|---|
| Hono+htmx | $2.67 | 8.6 min | 4.26M | 18 KiB |
| Astro | $2.76 | 8.2 min | 4.13M | 0 KiB |
| Nuxt | $2.82 | 9.0 min | 3.85M | 84 KiB |
| Vanilla (Vite) | $2.98 | 10.1 min | 5.00M | 7 KiB |
| Next.js | $3.14 | 10.9 min | 4.99M | 142 KiB |
| SvelteKit | $4.28 | 10.8 min | 8.53M | 36 KiB |
| Angular | $4.97 | 17.2 min | 9.76M | 93 KiB |

(Qwik, TanStack Start, SolidStart, React Router and Gracile+Lit are in the original.)

## Takeaways

- **Cost:** SvelteKit agents read the SvelteKit v3 migration guide every run (too new for training data); Gracile agents dug through the repo and compiled source; Angular's cost tracks its code size.
- **Quality:** Lighthouse Performance, Accessibility and CLS were near-perfect for nearly every stack, so differences show up in JS bytes, Total Blocking Time (Next.js 71 ms, Angular 103 ms, most others near 0) and LCP (MPA-style stacks win; Lighthouse is biased against SPAs).
- **Tooling use was patchy:** agents used a framework's skill or MCP in some runs and ignored it in others (for example, Nuxt's MCP was never used).
- **Caveats:** one app type, stochastic agents, wide per-stack ranges (cheapest React Router run beat the priciest Hono+htmx run).
