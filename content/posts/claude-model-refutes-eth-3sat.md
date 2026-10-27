---
title: "Anthropic says a Claude model found a 2^o(n) algorithm for 3-SAT, refuting ETH"
slug: "claude-model-refutes-eth-3sat"
date: 2026-10-27T16:02:54+0000
summary: "Anthropic posted a paper claiming a deterministic 2^o(n)-time algorithm for 3-SAT, which would refute the Exponential Time Hypothesis. The algorithm also solves the harder #3-SAT counting problem. Anthropic says an internal Claude research model found the core algorithm on October 16, using 14M output tokens and no human input, and that the result has been formalized in Lean 4. Researchers then worked with Claude on the presentation. The paper also notes that an internal OpenAI model independently claims a similar result. The claim is days old and not yet peer reviewed."
source: "https://hexagonmath.org/2610.00577v1"
source_title: "A 2^{o(n)} Time Algorithm for 3-SAT"
source_author: "Anthropic"
source_site: "Hexagon"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50255159"
---

**Bottom line:** Anthropic's paper claims to refute the Exponential Time Hypothesis (ETH), the widely assumed conjecture that 3-SAT needs time exponential in the number of variables. It presents a deterministic algorithm for 3-SAT running in 2^o(n) time, and it solves the harder problem of counting satisfying assignments (#3-SAT).

## Provenance

- The core algorithm was found on October 16 by an internal Claude research model, with 14M output tokens and no human input.
- Anthropic researchers then worked with Claude to improve the presentation, which produced the paper.
- The result has been formalized in Lean 4.
- The paper says an internal OpenAI model independently and concurrently claims a similar result.
- Hexagon labels the text "primarily AI-generated", with human understanding of the content marked as partial.

## Why it matters

ETH, and the stronger SETH, underpin many conditional lower bounds in algorithms, for example the result that edit distance cannot be computed in strongly subquadratic time unless SETH is false. If the claim holds, it would reshape a good part of fine-grained complexity. This summary is based on the abstract and provenance statement, not a check of the proof. The Lean formalization is the strongest support for correctness, but the community has not yet had time to examine it.
