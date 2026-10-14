---
title: "Deleting 168,000 lines of comments didn't change agent output quality"
slug: "code-comments-dont-help-ai-agents"
date: 2026-10-14T19:02:48+0000
summary: "An Automattic developer stripped every comment (about 168,000 lines) from a ~765k-line codebase and had Claude Code build the same two features three times on each version. All runs passed lint, type checks and tests, and judge scores were within noise. Token use dropped about 22% on the small task without comments, with no clear difference on the bigger one. Run-to-run variance was as large as the effect, so single runs would mislead."
source: "https://openstation.blog/2026/10/13/i-deleted-168000-lines-of-comments-my-ai-agent-didnt-notice/"
source_title: "I Deleted 168,000 Lines of Comments. My AI Agent Didn't Notice"
source_author: "Roberto Aranda"
source_site: "OpenStation"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50090484"
---
**Bottom line:** In this experiment, code comments neither helped nor hurt an AI coding agent. The author concludes you should keep or delete comments for human readers, while noting the test covers one model, two tasks and three runs per branch, enough to rule out a big effect but not a small one.

## Setup

Claude Code (Opus 5.5, high effort) implemented two real features on the original codebase and on a copy with all comments removed (verified to compile to identical code). A separate Claude session, blind to which branch each change came from, scored results against a checklist written beforehand. Averages of three runs:

- **Small task (tile windows, /19):** 18.3 with comments vs 19.0 without. Tokens 2.18M vs 1.71M; cost $1.42 vs $1.04.
- **Bigger task (pin window on top, /23):** 21.7 vs 22.0. Tokens 9.06M vs 9.12M; cost $3.84 vs $3.95.

The bigger task had a trap: pinned state must survive reload, and a PHP session-save function drops unknown keys. All six runs caught it, with or without comments.

## The variance lesson

On the bigger task, token use with comments ranged 8.21M to 10.17M and without ranged 7.01M to 10.64M. Picking one run per branch could "prove" either that removing comments makes the agent 30% costlier or 31% cheaper. The author's advice: measure agents more than once.

A follow-up post is promised on removing the repo's docs folder and AGENTS.md, which the author says did make a difference.
