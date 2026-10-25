---
title: "Serving a static blog to AI agents with .md copies, Accept negotiation, and llms.txt"
slug: "blog-markdown-llms-txt-for-agents"
date: 2026-10-25T12:02:41+0000
summary: "Jake Marsh made his static nginx blog agent-friendly in an evening, and the recipe is mostly build-time files plus a few lines of config: a markdown copy of every post at `<url>.md`, markdown served from the normal URL when the client sends `Accept: text/markdown`, an `llms.txt`, explicit robots.txt rules, and feeds that link to the markdown."
source: "https://deallocatedobjects.com/posts/making-my-blog-readable-by-ai-agents"
source_title: "Making My Blog Readable By AI Agents"
source_author: "Jake Marsh"
source_site: "Deallocated Objects"
source_date: "2026-10-24"
hn_url: "https://news.ycombinator.com/item?id=50225443"
---

**Bottom line:** Jake Marsh made his static nginx blog agent-friendly in an evening, and the recipe is mostly build-time files plus a few lines of config. His premise is that many of his readers are now agents (Claude, ChatGPT, Perplexity) fetching pages to answer someone's question, and HTML is noisy for them.

- **Markdown for every post.** Each post also exists at `<post-url>.md`, with YAML frontmatter (title, canonical URL, author, dates, description).
- **Content negotiation.** An nginx `map` on `$http_accept` plus a `rewrite` returns the `.md` file from the normal URL when the client asks for `text/markdown`. Responses send `Vary: Accept` so caches don't mix HTML and markdown. The `.md` responses carry a canonical `Link` header and `X-Robots-Tag: noindex` so Google doesn't see duplicates.
- **llms.txt.** A root markdown file with a blog summary, how to get markdown, feed locations, citation preferences, and every post with a one-line summary. He also publishes `llms-full.txt` (all posts, about 225 KB) and `/index.md`, and sends a `Link` header pointing to `llms.txt`.
- **robots.txt.** Allows everyone, names the major AI crawlers explicitly, and adds a `Content-Signal` line permitting search, AI input, and training. He wants to be cited; a product site might choose otherwise.
- **Feeds and meta.** Atom entries get summaries and a markdown alternate link; a JSON Feed adds a `_markdown_url` per item. Pages also got proper descriptions, canonical tags, Open Graph/Twitter cards with a 1200x630 image, JSON-LD (`BlogPosting`, breadcrumbs), and the `maximum-scale=1` viewport restriction was removed.
- **IndexNow.** A small script pings Bing and others on publish, which matters because Bing's index feeds many AI search tools.
