---
title: "AI agents built a Chrome full-chain exploit in six days for about $40,000"
slug: "ai-agents-chrome-full-chain-xbow"
date: 2026-10-13T21:02:44+0000
summary: "XBOW used parallel GPT-based coding agents, steered by human researchers, to build a Chrome exploit chain that goes from JavaScript in a tab to code running outside the renderer sandbox, with Calculator launching from a page in signed Chrome Beta. The build took six days (Aug 2-7, 2026) and about $40,000 in API charges, versus an estimated 6-9 months for a four-person human team. Google paid the first $250,000 Chrome full-chain reward under its April 2026 VRP rules."
source: "https://xbow.com/labs/research/zero-to-calc-in-six-days-inside-xbow-chrome-full-chain"
source_title: "Zero to Calc in Six Days: Inside XBOW's Chrome Full Chain"
source_author: "Brendan Dolan-Gavitt"
source_site: "XBOW"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50076503"
---

XBOW built a Chrome full chain with agents running OpenAI's `gpt-5.6-sol` in parallel campaigns: broad vulnerability search, V8 memory corruption, V8 sandbox escape, renderer sandbox escape, and integration. Agents searched source, tested hypotheses and wrote proofs of concept. Humans picked targets, supplied context, challenged results and decided which pieces to combine. Cost was roughly $40,000 in API charges, reconstructed from archived Codex logs. XBOW's first submission took two months overall.

## The three stages

1. **V8 / Maglev:** during deoptimization, Maglev materialized a mutable `HeapNumber` as a shared literal, so different reconstructed objects shared one mutable box. That gave read/write across V8's pointer-compression cage. The fix allocates a fresh box each time.
2. **Animated WebP (V8 sandbox escape):** transferring an `ArrayBuffer` to `ImageDecoder` left a stale in-cage view that could modify the bytes. Blink cached frame geometry from one WebP and decoded bytes from another, causing an out-of-bounds write: one chosen 64-bit write outside the cage.
3. **Crashpad (renderer sandbox escape):** after a failed read from a supposedly suspended renderer, Crashpad retried with a length that assumed memory protections hadn't changed. A freeze-exempt thread flipped page protections during the race, so the retry overflowed a heap slot and led to command execution in the unsandboxed handler.

The write-up also covers a hijack of V8's Wasm Code Pointer table and a roughly 70-byte loader built from 13 instruction islands.

## Takeaways from the authors

- Several bugs mutated state assumed to be fixed: a number box, a transferred buffer, a frozen process's memory.
- Collisions with other reporters are a real risk. An initial sandbox escape duplicated an existing report and was replaced by the WebP stage within hours, which the modular design made possible.
- They argue the cost and time of this class of exploit has dropped sharply, so the bar for attackers is lower. Their conclusion: "Defend accordingly."
