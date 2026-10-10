---
title: "What Mathematicians Should Know About Lean's Reliability in the Age of AI"
slug: "lean-reliability-ai-hales"
date: 2026-10-10T00:03:26+0000
summary: "Thomas Hales argues that Lean, now the dominant proof assistant, is trustworthy in practice but not yet on firm theoretical footing. AI autoformalization became real in 2026, yet the kernel had a 'Summer of Soundness Bugs', and basic metatheory such as a full public consistency proof for Lean's type theory is still missing. He urges humans to audit what AI has produced."
source: "https://terrytao.wordpress.com/2026/10/09/what-mathematicians-should-know-about-the-lean-theorem-prover/"
source_title: "What mathematicians should know about the Lean Theorem Prover: questions of reliability and AI"
source_author: "Thomas Hales (guest post on Terence Tao's blog)"
source_site: "What's new"
source_date: "2026-10-09"
hn_url: "https://news.ycombinator.com/item?id=50024090"
---

**Bottom line:** Thomas Hales argues that Lean is reliable enough for practical use, but its theoretical foundations are weaker than mathematicians assume, and as AI takes over more of the foundational work, humans need to audit it.

**Autoformalization is real.** AI turning papers into Lean proofs became practical in 2026. Milestones he lists include sphere packing in 24 dimensions, Anthropic's Fermat's Last Theorem formalization (13 million lines of Lean in 11 days), and OpenAI's Navier-Stokes blowup result. Mathlib, the main library, holds about 300,000 theorems and 2.5 million lines.

**What to trust.** A Lean proof only counts once the kernel, a few thousand lines of C++, has checked it. A human must also confirm that the formal statement matches the intended theorem. That is usually far easier than checking the proof.

**Summer of Soundness Bugs.** Several kernel bugs that could prove "False" surfaced in July and August 2026, including one that gave an illicit disproof of the Collatz conjecture. AI tools in the hands of security researchers found them, and they were fixed. Hales sees that as encouraging.

**Fixes:**
- Cross-check with other kernels (about 25 exist). This helps but is not enough, since the Collatz bug slipped past an older kernel that had its own bug.
- Formally verify the kernel. Joachim Breitner's Con-Leche, written with Claude, checks mathlib and comes with a consistency proof relative to set theory.
- Improve the theory. Lean's type theory still lacks a complete public relative-consistency proof, and basic properties such as unique typing remain unproved. An error was found in Mario Carneiro's foundational thesis.

**Warning.** Much of this work was handed to AI, so humans must audit it. A backdoored bug that AI hid in Lean or in Con-Leche is the Ken Thompson "trusting trust" scenario, and Hales asks what precautions exist against it. He calls for more type theorists to work on Lean's metatheory.
