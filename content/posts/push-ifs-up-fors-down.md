---
title: "Why \"push ifs up, fors down\" works, from Rust and SQL to category theory"
slug: "push-ifs-up-fors-down"
date: 2026-10-07T23:25:00-07:00
summary: "Put branching decisions in the caller and give hot loops branch-free, batched work. The same rule shows up as predicate pushdown in SQL planners and as a provable law in functional programming, and the math also tells you exactly when the rewrite is legal and when it actually saves work."
source: "https://debasishg.github.io/blog/push-ifs-up-fors-down/"
source_title: "Push Ifs Up and Fors Down: The Idiom, Its Algebra, and Its Limits"
source_author: "Debasish Ghosh"
source_site: "debasishg.github.io"
hn_url: "https://news.ycombinator.com/item?id=49997073"
---

Put branching decisions in the caller and give hot loops branch-free, batched work. The same rule shows up as predicate pushdown in SQL planners and as a provable law in functional programming, and the math also tells you exactly when the rewrite is legal and when it actually saves work.

## The idiom

It comes from TigerBeetle's "Tiger Style" guide and a well-known post by matklad. The guide's advice: keep all control flow in one parent function, and keep helpers free of branching.

- **Ifs up.** Instead of `frobnicate(walrus: Option<Walrus>)` unwrapping the option inside, make the caller handle `None` and pass a plain `Walrus`. The signature now documents what the function expects, and the callee has fewer cases to handle.
- **Fors down.** Instead of calling `frobnicate(walrus)` in a loop, write `frobnicate_batch(walruses)` with the loop inside. The hot loop has no branches, so it's a candidate for vectorization.
- **They combine.** The caller filters a `Vec<Option<Walrus>>` down to `Vec<Walrus>` and passes it to the batch function, which never sees a `None`.

## The database version

Query optimizers have done this for decades. The vocabulary is just upside down: in a query plan, data flows *up* from table scans, so "pushing a predicate down" means running it *earlier*.

- **Selections and projections run early.** `WHERE` filters and narrow column lists shrink the data before expensive operators see it.
- **Joins run late.** Joins are costly, so the optimizer feeds them the smallest inputs the query allows.
- **Vectorized execution is "fors down."** Old Volcano-style engines call each operator once per row. Vectorized engines call it once per batch of about a thousand rows and run a tight loop inside. Per-call overhead is paid once per batch.

## The category-theory version

- **Ifs up = restricting to a subobject.** A predicate on `A` picks out the subset of values that pass it. Moving the check to the caller means the callee's input type *is* that subset (`Walrus`, not `Option<Walrus>`), so it needs no `if`.
- **`Option<Walrus>` is a coproduct** (nothing, or a walrus). A function that branches on it is really two functions bundled together. Pushing the `if` up separates them: the caller handles the "nothing" case, and the core function only handles walruses.

## When "filter before map" is legal

"Filter before you map" is common advice, but `filter p (map f xs)` and `map f (filter p xs)` aren't the same thing: the predicate looks at different types. The law that actually holds is:

`filter p . map f == map f . filter (p . f)`

The author derives it by writing `filter` in terms of `Maybe`. The step that makes it work is that `catMaybes` is a natural transformation; `filter` itself is not.

**The rewrite doesn't automatically save work.** `filter (p . f)` still runs `f` on every element in order to test it. It only pays off when `p . f` simplifies to a cheap check on the *input*, typically because `p` only looks at a part of the value that `f` doesn't change.

## Rules for when each rewrite is valid

- **Hoisting an `if` out of a loop** is only valid if the condition doesn't depend on the loop element. A check that varies per element has to stay in the loop. The most you can do is move it to the function boundary and encode it in the type.
- **Pushing a filter below a join** is valid only if the predicate refers to one side of the join.
- **Filtering before mapping** is always legal in the `(p . f)` form, but only *cheaper* when `p . f` reduces to a cheap input predicate.

The author's takeaway: algebra tells you which rewrites are *legal*, while "fors down" is about *cost*. Turning `A -> B` into `[A] -> [B]` means paying setup costs once per batch instead of once per item.
