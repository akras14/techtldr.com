---
title: "tsc-ts: TypeScript 7 compiler ported back to TypeScript, built to native code"
slug: "tsc-ts-typescript-port-native"
date: 2026-10-17T20:02:36+0000
summary: "Vercel Labs published tsc-ts, an experimental port of the TypeScript 7.0.2 compiler (the Go-based tsgo) back into TypeScript, then compiled to a native executable with their scriptc tool. It's installable from npm and is checked against tsgo's output on TypeScript's own test corpus."
source: "https://github.com/vercel-labs/tsc-ts"
source_title: "tsc-ts: TypeScript written in TypeScript"
source_author: "Vercel Labs"
source_site: "GitHub"
hn_url: "https://news.ycombinator.com/item?id=50132911"
---

Vercel Labs released **tsc-ts**, an experimental TypeScript port of the TypeScript 7.0.2 compiler. TypeScript 7 is the Go-based rewrite, tsgo. tsc-ts is written as idiomatic TypeScript and compiled to a native binary with scriptc, Vercel's TypeScript-to-native compiler. It's labeled a Vercel Labs experiment.

**How it's verified.** Each stage is compared against tsgo on TypeScript's test corpus, about 12,500 files or 17,000 units once multi-file cases are split. The README reports full matches for the scanner, parser and AST, binder, module resolution, tsconfig and program handling, command line, diagnostics catalog (including 13 translations), JavaScript emit, declaration emit, `--build`/`--incremental`, and type checker diagnostics.

**Usage.** `npm install -D tsc-ts` then `npx tsc-ts -p tsconfig.json`. The package pulls a per-platform native executable through optional dependencies. It also implements `tsc --api` over stdin/stdout, so tools that drive that API, scriptc among them, can run tsc-ts in place of tsgo, with most language service methods supported. The CPU and heap profiling methods are not.

**Building it.** Needs Node 24+, Go 1.26 (for the tsgo reference builds) and clang for linking. The build is pinned to scriptc 0.3.5. The repo has a `bench` script that times it against tsgo and the JavaScript compiler, but the README gives no numbers.
