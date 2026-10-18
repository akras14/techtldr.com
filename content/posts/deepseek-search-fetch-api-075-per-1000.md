---
title: "DeepSeek adds Search and Fetch APIs at $0.75 per 1,000 requests"
slug: "deepseek-search-fetch-api-075-per-1000"
date: 2026-10-18T08:02:16+0000
summary: "DeepSeek now offers two raw JSON APIs for agents and RAG apps: Search (backed by its own search engine) and Fetch (returns a page as Markdown). Each costs $0.75 per 1,000 successful requests, billed per call rather than per token, with failed, empty, or timed-out requests free."
source: "https://api-docs.deepseek.com/guides/search/"
source_title: "Search & Fetch | DeepSeek API Docs"
source_site: "DeepSeek API Docs"
hn_url: "https://news.ycombinator.com/item?id=50138531"
---

DeepSeek now offers two raw JSON APIs for agents and RAG apps: Search, backed by its own search engine, and Fetch, which returns a single page as Markdown. Each costs $0.75 per 1,000 successful requests, billed per call rather than per token.

- **Search** (`POST /v1/tools/search`): takes a query of up to 128 characters and returns 1 to 50 results (default 10). Each result has a title, URL, short snippet, a 600 to 800 character model-oriented summary, optional full page content (`need_content`), a 0 to 3 relevance score, and a 0 to 4 authority tier. It supports operators such as `site:`, `-site:`, exact phrases, `lang:`, `after:` and `before:`.
- **Fetch** (`POST /v1/tools/fetch`): takes a URL and returns the title and content as Markdown. It can fail with 403 (security policy) or 404 (no usable content).
- **Auth and limits:** same bearer key as the main DeepSeek API; 5 QPS each for Search and Fetch, limited independently per account.
- **Billing:** only HTTP 200 responses with a result count. Failed requests, empty search results, and timeouts are free. `count` doesn't change the price.
- **Free for now in harnesses:** server-side tool calls made through DeepSeek Harness, Claude Code, or Codex already use these APIs and are free until billing starts at 00:00 Beijing time on November 1, 2026.
