---
title: "Gforth 1.0 adds multicore tasks, closures and a 2x loop speedup"
slug: "gforth-1-0"
date: 2026-10-27T12:08:10+0000
summary: "Gforth 1.0 is the first major release since 0.7.x. It documents which words are permanently supported, adds multi-tasking across cores with message queues, closures, a regex library and IDE-style tooling, and says an IP-update optimization speeds loop-dominated programs up to 2x, bringing it near SwiftForth."
source: "https://gforth.org/manual/1.0/NEWS.html"
source_title: "Gforth 1.0 NEWS"
source_site: "gforth.org"
hn_url: "https://news.ycombinator.com/item?id=50256952"
---

The NEWS file lists user-visible changes between Gforth 0.7.x and 1.0. The headline is a stability promise: the docs now state which words are meant to be supported permanently and which are not.

Main changes:

- **Text interpreter:** extensible with "recognizers" and new literal syntaxes (scaled floats like `6k5`, strings, xts, body addresses, `${ENV}` variables, vocabulary prefixes such as `forth:and`).
- **Concurrency:** multi-tasking on multiple cores or hardware threads, actor-style message queues, and Cilk-style work distribution.
- **Language features:** closures, quotations, multi-section dictionaries, a new word header layout with words to modify word behavior (`set-does>`, `set-optimizer`), per-task values and defers, and a regular-expression library.
- **Tooling:** source browsing (`locate`, `where`), browsable backtraces that open an editor, a "smart" `.s`, a status line showing base, stacks and search order, code coverage, and a disassembler.
- **Interop and libraries:** C interface improvements including callbacks and C++ libraries, string handling with automatic memory management, CSV input, and the MINOS2 GUI toolkit.
- **Performance:** the authors claim performance comparable to SwiftForth, with an IP-update optimization speeding loop-dominated programs up to 2x, plus a literal stack enabling constant folding.

Several older words are no longer documented (for example `.name`, `name>int`, `sl@`) and may be removed later; the file names replacements.
