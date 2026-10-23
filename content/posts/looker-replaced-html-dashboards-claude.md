---
title: "Replacing a $150k Looker with Claude-written HTML dashboards for $8.3k"
slug: "looker-replaced-html-dashboards-claude"
date: 2026-10-23T19:02:25+0000
summary: "A three-person data platform team at a ~500-person company built an internal host for Claude-generated HTML dashboards, and in eight months it matched Looker's weekly users while Looker usage fell by a third. They report spending about $8.3k (roughly $6.4k in Claude tokens, $1.9k AWS) versus a ~$150k/year Looker licence, though some teams still use Looker and some dashboards have no replacement yet."
source: "https://david.lequin.dev/blog/bi-software-is-dead/"
source_title: "Can we replace a $150k Looker with a bucket of HTML files?"
source_author: "David Lequin"
source_site: "david.lequin.dev"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50212320"
---

The team's bottom line: once Claude could query the warehouse and write polished single-file HTML pages, the parts of Looker they were paying for (chart UI, permissions, semantic layer, hosting) were mostly already covered by Snowflake roles, dbt models and an S3 bucket. Eight months in, they estimate the home-built tool cost about $8.3k against roughly $100k of Looker licence over the same period, and running it should be about $20k a year versus $150k.

**How it grew**
- It began as an afternoon project in February to host HTML files that Claude produced. A live query API using the viewer's own Snowflake OAuth token followed in March, so Snowflake's existing roles decide who sees what and no new permission model was needed.
- Weekly viewers went from 23 in March to 136 in June. Looker usage fell by about a third, but some teams still use it.
- The first version had weak security: dashboard JavaScript ran same-origin with the user's session and every employee could read every dashboard. They rebuilt it rather than keep that.

**The rebuilt design**
- One remote MCP connector for the whole company (claude.ai, Claude Code, Slack, their analyst agent, CI). AI uploads land as drafts and a human must click Publish.
- Every dashboard is treated as hostile: it runs in a sandboxed iframe on its own origin with no cookies and a strict CSP, talks to the parent only via `postMessage`, and queries run as the viewer. Content tokens last an hour and are scoped to one dashboard, viewer and host.
- A separate async service extracts each version's scripts, queries and outbound destinations and applies rules. Most rules only report; a few (an unfamiliar package, WebRTC) make the viewer accept a warning before the page runs.
- Content management copies Google Drive's model of folders, grants and versions.

**Costs reported**
- 325 commits, about 74,000 lines (20,000 of them tests), written mostly by Claude with human review.
- Their analyst agent uses about 1M tokens per dashboard; they say that dropped from roughly $2.50 to $0.50 after moving to Opus 5.5.
- Ongoing: about $6k/year AWS plus roughly $1.3k/month of Claude spend on new features.

These figures are the author's own, from one company with a mature dbt and Snowflake setup, so the result may not carry over to teams without that foundation.
