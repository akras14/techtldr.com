---
title: "Claude beat published designs on 8 quantum circuit tasks, some by 10x or more"
slug: "claude-quantum-circuit-designs"
date: 2026-10-16T19:03:05+0000
summary: "Anthropic gave Claude plain-language specs for eight quantum circuit primitives plus an automated evaluator that checks correctness and scores qubits times gates. Working in a closed loop with little human input, it found designs that improve on the best published ones, in some cases by an order of magnitude or more. One example is a new reversible approach to modular inversion for elliptic-curve point addition, found after 53 hours of work."
source: "https://www.anthropic.com/research/designing-efficient-quantum-circuits"
source_title: "Designing more efficient quantum circuits with Claude"
source_author: "Anthropic"
source_site: "anthropic.com"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50121939"
---

**Bottom line:** Anthropic reports that Claude Mythos 5, set up with a plain-language spec and a programmatic evaluator, designed quantum circuits that use fewer qubits and fewer gates than the best published ones across eight primitives (chemistry, optimization, simulation, number theory, numerical computing). The company says this could bring practical quantum computers sooner than expected; that is its own claim, and the full results are in an arXiv paper.

- **Setup:** circuits are written as Python, an evaluator checks each one on classical hardware (these circuits give one definite output per input), and scores it by qubits times gates. Claude proposes, gets the score, and revises, with no human input between rounds.
- **Why it works:** the improvements came from new structural and mathematical insights, not parameter tuning, such as reusing results that symmetries make identical, or removing costly components that only handle extremely rare inputs.
- **Point addition:** for elliptic-curve point addition, a standard benchmark, Claude got a one-paragraph brief and worked for 53 hours with minimal human input. It came up with a new reversible way to do modular inversion, the costliest step, which avoids the long record of branch information that a naive quantum version of divsteps would have to keep in qubits.
- **Second result:** a leading quantum optimization algorithm's costliest step became about 12x cheaper.
- **Credit:** led by Andi Gu in Anthropic's STEM Fellows Program, with Harvard and QuEra collaborators.
