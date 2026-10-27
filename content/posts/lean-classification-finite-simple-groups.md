---
title: "Classification of finite simple groups formalized in Lean: 43M lines"
slug: "lean-classification-finite-simple-groups"
date: 2026-10-27T18:02:58+0000
summary: "Axiom Math has published a complete Lean formalization of the classification of finite simple groups, following the first-generation proof, with every step checked by Lean's kernel. The project spans 43.1M lines of Lean and 2.05M declarations, draws on 715 papers and books, and uncovered an issue in a published argument along the way."
source: "https://cfsg.axiommath.ai/"
source_title: "CFSG in Lean"
source_site: "cfsg.axiommath.ai"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50261647"
---

**Bottom line:** The classification of finite simple groups (every such group is cyclic of prime order, alternating, of Lie type, or one of 26 sporadic groups) now has a machine-checked proof in Lean. The first-generation proof spans thousands of journal pages by about a hundred mathematicians over decades.

- **Scale:** 43.1M lines of Lean, 2.05M declarations, 100 milestones, drawing on 715 papers and books. The page includes an interactive proof map, a corpus view and an agent blueprint, the initial plan given to the agents that worked on it.
- **Errata found:** while formalizing, the team found counterexamples to a closure assertion used in a published route to the Unbalanced Group Theorem. A companion paper (Hexagon 2610.00575, 35 pages) explains the obstruction and gives an alternative argument for the simple-group case based on Solomon's 1976 theory of maximal 2-components. They are compiling further errata with group theorists.
- **People:** core contributors are Dean Cureton, Vasily Ilin, Bhavik Mehta and Niels Voss, with many more mathematical and engineering contributors.

Why it matters to developers: this is a very large, agent-assisted formal verification project, and the formalization itself surfaced errors in the human literature.
