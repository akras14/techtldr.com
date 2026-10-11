---
title: "Valen's borrow checker lets multiple refs write to one object, safely"
slug: "valen-group-borrowing"
date: 2026-10-11T14:04:00+0000
summary: "Evan Ovadia presents the new borrow checker in his language Valen, built on Nick Smith's Group Borrowing proposal. The compiler remembers which group of objects each reference points into, and relaxes Rust's \"shared XOR mutable\" rule to \"no use-after-free\". That lets a program hold several read-write references to the same object, while still rejecting a reference that outlives a reassigned or freed owner at compile time, with no run-time cost."
source: "https://verdagon.dev/blog/valen-group-borrowing"
source_title: "Valen's Memory Safety: A New Kind of Borrow Checking"
source_author: "Evan Ovadia"
source_site: "verdagon.dev"
source_date: "2026-10-11"
hn_url: "https://news.ycombinator.com/item?id=50042362"
---

**Bottom line:** Valen's borrow checker tracks where each reference points, and uses that to enforce "no use-after-free" instead of Rust's stricter "shared XOR mutable". The author claims this gives compile-time memory safety with no run-time cost, while allowing patterns that borrow checkers normally reject.

**What it accepts.** According to the post, the following are allowed:
- Several local variables pointing at the same object, with writes through any of them. The example pushes to one `Vec` through two references.
- A parameter pointing at an object plus another pointing inside it, writing through only the latter.
- Multiple function parameters pointing at one object.
- "Rollback" structs that change an object when they go out of scope, like the ScopeGuard pattern.
- Multiple structs holding mutable references to a shared subsystem.

**What it rejects.** In the example, a reference to `world.entities[0]` is taken, then `world.entities` is replaced with a new `Vec`, then the old reference is used. This fails with "Used a borrow after invalidated".

**Background.** The approach comes from the "Group Borrowing" proposal by Nick Smith, which the author and Smith wrote up last year after it was passed over for Mojo. The author says other languages picked it up: Carbon, Ante, Plecra's exclusion typing for Rust, Zeta and Fang. Valen combines it with Vale's generational-reference approach and aims to interoperate with Rust code.

**Scope.** The post covers the relaxed borrow checker, which is the first of three problems the author lists. The other two are reading complex, graph-shaped data and making both systems work together. I read only the first part of this very long post, so treat the details of how it works as unsummarized. The author says some syntax shown is sugar and some is coming in November.
