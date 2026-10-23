---
title: "Zig traits as comptime structs of functions, no library needed"
slug: "zig-traits-comptime-structs"
date: 2026-10-23T13:05:00+0000
summary: "Axel Magnusson argues Zig can express Rust-style traits in plain userland: a trait is a comptime struct of functions over a type, passed explicitly to the code that needs it. Unlike vtable interfaces, the compiler can monomorphize and inline it, so hot loops stay fast; unlike anytype duck typing, the contract is explicit and shows up in autocomplete. The pattern resembles OCaml's first-class modules and functors rather than Rust's globally resolved traits."
source: "https://www.axelmagn.com/blog/202610-zig-traits/"
source_title: "We have Traits at Home"
source_author: "Axel Magnusson"
source_site: "axelmagn.com"
source_date: "2026-10-03"
hn_url: "https://news.ycombinator.com/item?id=50202482"
---
Axel Magnusson argues that Zig can express Rust-style traits in plain userland, with no library. A trait is a comptime struct of functions over a type, passed explicitly to code that needs it. It sits between the two usual options: vtable interfaces, which cost indirection and block compiler optimization, and `anytype` duck typing, whose contract lives only in documentation.

**The pattern**
- `fn Num(T: type) type` returns a struct with `add`, `sub`, `mul` and `div` function fields. A generic function such as a Möbius transform takes `num: Num(T)` as an argument.
- Types usually declare their implementation as a const on themselves (`Complex.num`). Primitives get one from a helper like `builtinNum(T)`, which switches on `@typeInfo`.
- The compiler sees through all of it, so it monomorphizes and inlines the calls. The author prefers this to runtime polymorphism in hot loops and in library code that shouldn't dictate data layout.

**Helpers**
- `impl(Trait, struct{...})` builds a trait value from an anonymous struct and removes boilerplate.
- `getTrait(Trait, Impl)` finds a trait declared on a type, bringing back duck-typed convenience when you want it.
- Trait functors take a trait and return another trait or a specialized function, the same way OCaml module functors do. The post also covers traits such as Into/From, Index and Iterator.

**Why not Rust or Haskell traits**
Magnusson compares the pattern to OCaml modules: implementations are first-class values passed explicitly, never inferred from a global registry. That fits Zig's explicit-control-flow principle and avoids hunting for where a trait is defined, though it costs some verbosity.

**Limitations**
- A trait must be parameterized by a known type. Zig has no associated types, so `Iter(T, Elem)` needs both types up front.
- Traits don't combine well with type erasure. The author has not found an ergonomic workaround.
