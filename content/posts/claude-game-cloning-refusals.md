---
title: "Claude Code refusing to recreate existing games, and some reverse-engineering"
slug: "claude-game-cloning-refusals"
date: 2026-10-31T19:02:27+0000
summary: "Tom's Hardware reports users are hitting Claude Code and model refusals on tasks that recreate existing games and on some ordinary reverse-engineering, apparently tied to a rule not to build \"a recreation of a specific existing game, even if it is renamed.\" The rule text is unconfirmed, and Anthropic has made no statement. Developers report false positives, including anti-cheat work and crash-dump analysis."
source: "https://www.tomshardware.com/tech-industry/artificial-intelligence/anthropics-ai-model-is-now-blocking-attempts-at-video-game-recreation-and-cloning-users-also-claim-non-malicious-generic-reverse-engineering-tasks-are-blocked"
source_title: "Anthropic's Claude AI blocks video game cloning and reverse-engineering"
source_site: "Tom's Hardware"
hn_url: "https://news.ycombinator.com/item?id=50315987"
---

**Bottom line:** Over the past week or so, users report Claude Code and Opus 5.5 refusing game-related and reverse-engineering work, possibly because of a new usage rule. Tom's Hardware stresses the exact rule text is unconfirmed and Anthropic has not commented.

- **The rule (alleged):** "Do build original games, games in a genre, and games from the user's own designs and assets. Don't build a recreation of a specific existing game, even if it is renamed or called original." It comes from a Twitter user, not from Anthropic.
- **Reports:** Claude Code's GitHub has new issues about refusals mid-development, when replicating a game mechanic, and when patching a tabletop-like game. Others report blocks on analyzing a Windows crash dump, cleaning junkware off a Samsung TV, using the Reverse Engineer Anything tool, and anti-cheat and security reviews.
- **Possible causes:** a wave of AI-made game decompilations and clones, including extortion threats against small studios, and caution about lawsuits from software companies. The refusals may also relate to Anthropic's Cyber Verification Program, which limited advanced cyber capabilities on October 6, though the timing doesn't line up cleanly.
- **Wider context:** OpenAI reportedly deactivated an account for someone cleaning up TV software. Commenters suggest local or open-weight models as a workaround.
