---
title: "Valen's borrow checker walks each function backwards to find use-after-free"
slug: "valen-reverse-borrow-checker"
date: 2026-10-19T02:02:06+0000
summary: "Valen, an experimental language, runs its memory-safety check as a backward scan from a function's last statement to its first. The scan keeps a pending list of variables used later, and flags any mutation that would invalidate one. The author says this simplifies last-use bookkeeping and loop handling, and he expects it to be faster, though he hasn't benchmarked it. The checker is about 3,600 lines and still lacks break/continue, closures and more."
source: "https://verdagon.dev/blog/reverse-borrow-checking"
source_title: "A New Way to Think About Borrow Checking (In Reverse)"
source_author: "Evan Ovadia"
source_site: "verdagon.dev"
source_date: "2026-10-18"
hn_url: "https://news.ycombinator.com/item?id=50140383"
---

**Bottom line:** Valen's new borrow checker does its safety check in reverse. It walks each function from the last statement up to the first, which the author believes no other borrow checker does. He is asking for feedback because there may be a good reason nobody has tried it.

**How it works.** The checker first types the function, then does a forward "grouping" pass that records which variable or part of a variable each reference points into. The safety pass then scans backwards and tracks a `pending` list of variables that are used later:

- **USE** of a variable: add it to `pending`.
- **MUTATE** of a group: if anything in `pending` points into that group, report a use-after-free error.
- **DECLARE** of a variable: remove it from `pending`, since nothing earlier needs it.

In the post's example, calling `e.add_buff()` between taking `&e.buffs[0]` and using it is rejected. Moving the call above the borrow passes.

**Why backwards.** The author gives three reasons:

- Finding a value's last use is easy when scanning in reverse, which helps with bookkeeping such as destructor insertion.
- Loops are simpler. A forward scan would have to look ahead to see whether a variable is re-assigned later in the loop body. The reverse version keeps per-loop `mutations_after_this` and `revived_after_this` lists and checks them at the loop start.
- Each node is visited once per pass, so he expects it to be faster. He says this is unbenchmarked, and the mutation check against `pending` is not yet O(n).

**Status.** It is a work in progress, and Valen is still experimental, with v0.1 planned for around February. The checker is about 3,600 lines and still lacks `break`/`continue`, closures and associated groups. The author also wants to replace the forward grouping pass with a Rust-style constraint solve, which he says would not require changing the backward pass.
