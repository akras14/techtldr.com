---
title: "zt: Templ-style HTML templates compiled to allocation-free Zig"
slug: "zt-type-safe-html-templates-zig"
date: 2026-10-12T06:02:51+0000
summary: "Lukáš Lalinský released zt, a Zig port of Go's Templ. Templates are transpiled to plain Zig during zig build, so a typo like {u.nmae} or a wrongly typed argument is an ordinary compiler error. The generated code only writes to a std.Io.Writer, with no allocations, and values are HTML-escaped by default."
source: "https://lalinsky.com/2026/10/11/type-safe-html-templating-in-zig.html"
source_title: "Type-safe HTML templating in Zig"
source_author: "Lukáš Lalinský"
source_site: "lalinsky.com"
source_date: "2026-10-11"
hn_url: "https://news.ycombinator.com/item?id=50051573"
---

**Bottom line:** zt is a templating library for Zig, adapted from Go's Templ. The author wanted templates that are type-checked at compile time, fast, safe against XSS by default, and still look like HTML. Nothing he found met all four.

## How it works

- Templates are parsed and transpiled to Zig during `zig build`. Templates can import app code, and app code can import templates, so every parameter is fully typed.
- Generated code is a sequence of writes to a `std.Io.Writer`, with no allocations. It can write to a socket or a buffer.
- Template mistakes surface as Zig compiler errors. A misspelled field gives "no field named 'nmae' in struct 'models.User'", and each generated line carries a comment pointing back to the template location.
- Control flow (`if`, `for`, `switch`, payload captures) is plain Zig. Components can be passed around as values, and `@children` handles layouts.

## Safety

- All dynamic values are HTML-escaped in text and attributes. URL attributes such as `href` and `src` are also checked for dangerous schemes.
- Raw output needs an explicit `{!value}`, which is easy to spot.
- The parser requires properly closed elements. The author notes this removes the most common XSS sources but doesn't cover everything.

## Caveats

- The project is young, though several people already use it.
- Block-level `if`/`for`/`switch` lines are parsed as code, so prose starting with "if you forgot..." on its own line is a parse error. The author is considering `@if`/`@for`/`@switch` and is asking for feedback on GitHub.
- It integrates with the author's Dusty web library (`res.render(.html, pages.HomePage, .{user})`), which has an htmx-based TODO example.
