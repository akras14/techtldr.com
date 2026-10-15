---
title: "7,959 of 217M web responses carried AI-directed HTTP headers; 30 tried injection"
slug: "prompt-injection-http-headers"
date: 2026-10-15T16:02:41+0000
summary: "Signals Corps scanned the response headers of 217 million Common Crawl pages (10% of September 2026's crawl) and found 7,959 sites sending headers aimed at AI agents. Nearly all are harmless pointers to llms.txt or MCP servers; only 30 attempted prompt injection, mostly pranks."
source: "https://www.signalscorps.com/blog/prompt-injection-in-http-headers/"
source_title: "Prompt injection in HTTP headers"
source_site: "signalscorps.com"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50104310"
---

Headers are a prompt-injection route most research ignores: a browser never shows them, but an agent's fetch tool may pass them straight to the model. Whether anything reaches the model depends on which parts of the response your tool forwards, so the defense starts there.

What the scan found across 217,097,223 responses:

- **7,959 sites** sent an AI-directed header; 7,271 were pointers, 6,989 of them `x-llms-txt`. Treat those destinations as new, untrusted fetches.
- **30 sites** sent injection attempts, 24 in a header named `X-AI`. 22 were pranks; the few asking for real actions only work if the agent has matching powers like a shell or database access.
- **139 sites** sent a header recording whether they classified the client as an AI bot (136 said yes).
- Some sites use headers to push back on agents, including strings modeled on Anthropic's test triggers.
- Name-matching alone would have wrongly flagged 910 sites, so the authors suggest alerting on what the agent does after a fetch.

In the authors' October 2026 tests, Claude Opus, Sonnet and Haiku in Claude Code saw the test headers and did not act on them by default, but obeyed in 7 of 9 runs when the user's prompt told them to follow the site's directions. How much authority your prompts give a site matters.
