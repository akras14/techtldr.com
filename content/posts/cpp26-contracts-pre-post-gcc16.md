---
title: "C++26 contracts: pre, post and contract_assert, working today in GCC 16"
slug: "cpp26-contracts-pre-post-gcc16"
date: 2026-10-15T15:02:44+0000
summary: "C++26 contracts let you put a function's requirements in its declaration with pre(...) and post(result: ...), plus contract_assert inside bodies. Unlike assert, how a check behaves (ignore, observe, enforce, quick-enforce) is chosen by the implementation, not NDEBUG. As of October 2026 only GCC 16 supports them, via -fcontracts."
source: "https://www.cppstories.com/2026/cpp26_contracts/"
source_title: "C++26 Contracts: Introduction to Preconditions and Postconditions"
source_author: "Bartlomiej Filipek"
source_site: "C++ Stories"
source_date: "2026-10-11"
hn_url: "https://news.ycombinator.com/item?id=50103164"
---

**Bottom line:** C++26 contracts move a function's requirements out of the body and into its declaration. `pre(index < values.size())` states what the caller must provide, and `post(result: result >= low && result <= high)` states what the function promises on normal return, covering every return path with one check. `contract_assert` handles conditions inside a body. As of October 2026 only GCC 16 implements them (`g++ -std=c++26 -fcontracts -fcontract-evaluation-semantic=enforce`).

**How it differs from `assert`**
- Requirements are visible to anyone reading the API, even when runtime checking is off.
- Instead of the all-or-nothing `NDEBUG`, there are four evaluation semantics: ignore, observe (report and continue), enforce (report and terminate), and quick-enforce (terminate without the handler). The implementation picks which applies; C++26 has no portable way to force one per assertion.
- With observe, execution continues after a violation, so an invalid access can still be undefined behavior.

**Details that can surprise you**
- Contracts are set by a function's first declaration; later declarations may omit them or repeat them identically, but can't change them. They aren't part of the function type.
- A by-value parameter used in a postcondition must be `const` on every declaration.
- Predicates see their variables as const (`pre(++count > 0)` fails to compile), but a predicate can still call a function with side effects. Predicates shouldn't change program state, since checks may be skipped.
- Postconditions aren't checked when the function exits via an exception.
- GCC lets you replace the violation handler (receiving a `std::contracts::contract_violation`), but that's implementation-defined, and what happens after the handler returns doesn't change.
- Contracts aren't input validation. Expected failures, such as a bad index from a config file, still need ordinary error handling; contracts document internal requirements.
