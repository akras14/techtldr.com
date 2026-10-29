---
title: "Claude Code's agent status summaries cost 16% of one team's spend"
slug: "claude-code-agent-summary-token-cost"
date: 2026-10-29T05:02:47+0000
summary: "Flamerix AI says Claude Code spends a large share of its tokens on a feature that only refreshes a short status line in the agents panel. Every ~30 seconds, each background agent sends its entire transcript back to its own model (mostly Opus) to produce a 3-5 word description of its latest action. In their OpenTelemetry data, these `agent_summary` requests were 16.4% of Claude Code spend (more than the main conversation's 15.7%) and 19.7% of API requests. They fell to 0% after the authors installed a Bun preload hook that suppresses the timer."
source: "https://gist.github.com/flamerix-ai/c3905705cc4205e5de3011fa2e74efce"
source_title: "Claude Code burns ~15–20% of your tokens on a 3-word status line. Here's how to turn it off"
source_author: "Flamerix AI"
source_site: "gist.github.com"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50284255"
---

Flamerix AI says Claude Code spends a large share of its tokens on a feature that only refreshes a short status line in the agents panel. Every ~30 seconds, each background agent sends its entire transcript back to its own model (mostly Opus) to produce a 3-5 word description of its latest action. In their OpenTelemetry data, these `agent_summary` requests were 16.4% of Claude Code spend (more than the main conversation's 15.7%) and 19.7% of API requests. They fell to 0% after the authors installed a Bun preload hook that suppresses the timer.

## Details

- **Default since 2.1.232:** the feature has been on by default since August 2026. The authors say no setting disables it. The documented SDK option `agentProgressSummaries` can only turn it on, the interval comes from a server flag clamped to 30-240 s, and the env vars that do stop it (`CLAUDE_CODE_FORK_SUBAGENT=0`, `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=1`) also disable fork subagents or background agents.
- **Hard to see:** the requests are not written to session logs on disk, so tools that read transcripts miss them. Only the OpenTelemetry export shows them. Others have reported 18-26% in a linked GitHub issue (#86879).
- **Cache helps, not enough:** about 98% of these calls re-read the transcript from prompt cache, but the ~2% on a cold cache carry over a third of the cost. On a subscription, the loss is rate-limit headroom rather than dollars.
- **The workaround:** `no-agent-summary.js` wraps `setTimeout`/`setInterval` and swaps the summarizer's timer for an inert one, matching it by a string in the callback's source. It is loaded via `BUN_OPTIONS=--preload`, and the binary is not modified. The daemon must be restarted after installing.
- **Caveats:** tested on Linux with Claude Code 2.1.257-2.1.312; macOS and Windows are untested. Matching is by source text, so an update can silently undo it, and the authors recommend re-checking after every update. The visible side effect is that the per-agent panel status stops updating.
- **Possible upstream fix:** version 2.1.310 ships a `short` mode (about 2k tokens of notes instead of the full transcript), but it is off by default and controlled by server flags.
