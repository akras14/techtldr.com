---
title: "Parser combinators in TypeScript: a ping-output parser from three helpers"
slug: "parser-combinators-typescript-ping"
date: 2026-10-18T11:02:41+0000
summary: "A short TypeScript tutorial builds a parser library from one type, Parser<T> = (input) => [value, rest], and three combinators: pSeq, pMap and pRepeat. It composes them into a parser for real ping output, as a readable middle ground between regex (messy at scale) and tree-sitter (overkill)."
source: "https://t3.at/blog/2026-10-17-parser-combinators/"
source_title: "Parser Combinators"
source_author: "Flo"
source_site: "t3.at"
source_date: "2026-10-17"
hn_url: "https://news.ycombinator.com/item?id=50139067"
---

**Bottom line:** A parser is just a function that consumes part of a string and returns a value plus the leftover input. Compose a few of those and you can parse structured text without regex soup or a full parser generator.

- **Core type:** `Parser<T> = (input: string) => [T, string]`. Failure is signaled by throwing.
- **Primitives:** small parsers for integers, decimals, strings, whitespace, semicolons and IPv4 addresses, built on regex or literal matching.
- **Combinators:** `pSeq` runs parsers in order and returns a typed tuple, `pMap` reshapes a result (for example into an object), and `pRepeat` applies a parser until it fails, like regex `*`.
- **Capstone:** real `ping` output is parsed into a nested object (header, packets, statistics) using only those three combinators.

The author argues this is easier to keep correct and maintainable than nested regex, and notes that backtracking and lookahead are possible because parsers are plain composable functions. That last claim is asserted rather than shown in code. It is an introductory treatment, but the code is complete and runnable.
