---
title: "Eurydice compiles Rust to readable C, but only small programs so far"
slug: "eurydice-rust-to-c"
date: 2026-10-10T03:02:38+0000
summary: "Eurydice is a new compiler that turns Rust into structure-preserving C, aimed at high-assurance projects whose verification and compliance tools only understand C. LWN's Daroc Alden finds it works well on small, self-contained programs but doesn't yet scale, mostly because its Charon front end chokes on newer Rust features."
source: "https://lwn.net/Articles/1055211/"
source_title: "Compiling Rust to readable C with Eurydice"
source_author: "Daroc Alden"
source_site: "LWN.net"
source_date: "2026-01-30"
hn_url: "https://news.ycombinator.com/item?id=50027853"
---
Eurydice, part of the Aeneas verification project (maintained by people at Inria and Microsoft), converts Rust to C while keeping the shape of the original code. Unlike rustc, which emits machine-oriented code, it aims for output that existing C verification tools can consume. It has already been used on some post-quantum-cryptography routines.

How it works: Charon extracts rustc's MIR as JSON, Eurydice converts it to the KaRaMeL intermediate representation (the same backend used for F*), and small passes strip Rust-only constructs before C is emitted. Evaluation order is preserved with extra temporaries, so the output is more verbose than the source but structurally similar.

Not everything maps cleanly. Iterator loops become while loops backed by support code, generics are monomorphized into duplicate functions, and dynamically sized types need two C representations to avoid adding or dropping bounds checks. Converting between them breaks strict aliasing, so the author of Eurydice recommends building with `-fno-strict-aliasing`.

The limits: Alden's tests found Charon was routinely foiled by newer features such as const generics, so Eurydice suits small programs. Those are also the easiest to rewrite by hand, so it pays off mainly when the Rust will keep evolving and you want the C kept in sync automatically.
