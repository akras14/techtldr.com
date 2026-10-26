---
title: "Pharo 14 adds RISC-V JIT, new dev tools and 978 merged PRs"
slug: "pharo-14-rise-of-new-tools-and-risc-v-jit"
date: 2026-10-26T15:03:05+0000
summary: "Pharo 14, the Smalltalk-style live programming environment, is out. It rebuilds the development tools, refactoring infrastructure and virtual machine, and adds a RISC-V JIT. It is also the first release under a new fall schedule."
source: "https://pharo.org/news/2026-10-26-Pharo14Release.html"
source_title: "Pharo 14 Released!"
source_site: "pharo.org"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50241925"
---

Pharo 14, the pure object-oriented language and live environment, has been released. The release effort centered on three areas, and it is the first to ship on a new fall (September–November) schedule instead of spring.

**Tools.** New Mission Control and Pulse (a Spotter replacement), a reworked Method Browser, a rewritten Settings Browser, and Epicea and ProfStef folded into NewTools. This continues the migration of the tooling to the Spec-based stack.

**Refactoring and commands.** Refactoring commands moved to a driver-based architecture behind a unified command registry. Command-line abilities grew: command-line testing, a command-line debugger and Sindarin script tooling.

**Virtual machine.** Unified frames and stack management, RISC-V JIT support, direct old-space allocation and GC improvements, more Slang/C compilation work, and modernized cross-platform builds.

The team reports 978 pull requests merged in the main Pharo repository and 616 issues closed since Pharo 13. The announcement is a feature list with no benchmarks or migration notes.
