---
title: "2,000 agents rewrote Prime Agent in Rust; cold start fell to 52 ms"
slug: "prime-agent-rust-rewrite"
date: 2026-10-10T02:02:56+0000
summary: "Prime Intellect says Prime Agent orchestrated a swarm of more than 2,000 agents over two weeks to rewrite itself from TypeScript to Rust, using differential parity tests as the objective check. A follow-up benchmark-driven optimization loop cut cold-start latency from 736 ms to 52 ms and memory use by about 4.8x."
source: "https://www.primeintellect.ai/blog/prime-agent-rust"
source_title: "Rewriting Prime Agent in Rust"
source_site: "Prime Intellect"
hn_url: "https://news.ycombinator.com/item?id=50027694"
---

**Bottom line:** Prime Intellect (by its own account) used Prime Agent, its coding agent, to rewrite itself from TypeScript to Rust. Over two weeks, a swarm of 2,000+ agents ran across 10,000+ sandboxes and about 200 billion tokens from a GLM-5.3 endpoint. The company reports large gains in startup speed and memory, plus Windows support and per-session crash isolation. Humans stayed in the loop for later bug finding, direction and review.

## How it worked

- **Verification first.** The humans' main job was building automatic parity checks: a differential test suite comparing terminal frames of the old and new binaries, comparing session transcripts and model requests, checking daemon protocol message types, and agent audits of each component for feature parity.
- **Orchestration.** One root agent wrote no product code. It split work into a dependency-ordered task list. Each task went through a planner, an implementer (in its own worktree), an adversarial reviewer on a different model, and a verifier that ran tests in a fresh sandbox. Failures went back to the implementer.
- **Limits of the checks.** The authors note parity tests only verify what they exercise. Further bugs surfaced when the team dogfooded the Rust build, and release quality took weeks of follow-up.

## Results the authors report

| Metric | TypeScript | Rust (after tuning) |
|---|---|---|
| Cold start to input-ready | 736 ms | 52 ms (14x faster) |
| Warm start | 552 ms | 41 ms (13x faster) |
| Memory, 10 MiB session | 1,130 MB | 237 MB (4.8x smaller) |
| Install size | 172 MB | 60 MB |
| Agents view switch | 78 ms | 13 ms |

A three-day optimization loop with no numeric targets logged 144+ experiment and audit records and merged 69+ changes, mostly moving work off startup and render paths, replacing polling with event-driven waits, and freeing memory sooner. The rewrite also split the code into nine crates, with the largest file down from about 15,000 to about 2,500 lines.

These figures come from the vendor's own benchmark harness.
