---
title: "curl 8.12–8.16 config files could simulate any two-counter machine"
slug: "curl-accidentally-turing-complete"
date: 2026-10-30T17:02:40+0000
summary: "Nicolas Seriot shows that from curl 8.12 through 8.16, variable slicing plus unbounded config-file includes were enough to encode any two-counter Minsky machine, making curl's config language Turing-complete in theory. The five-level include cap in 8.17 breaks the construction. There is no security impact."
source: "https://seriot.ch/computation/curl.html"
source_title: "How curl was accidentally Turing-complete for nine months"
source_author: "Nicolas Seriot"
source_site: "seriot.ch"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50300011"
---

**Bottom line:** Two curl features, byte-range slicing of variables (added in 8.12) and nested `expand-config` includes with no depth limit, were together enough to simulate any two-counter Minsky program. Curl 8.17.0 added a five-level include cap that defeats it. The author reports no security impact.

**The construction.** A counter's value is the number of `x` bytes in a variable. Increment appends an `x`; decrement slices off the first byte; a jump-if-zero slices out the first byte and includes `state_{{t}}.conf`, so config file names act as instruction labels. A four-file proof of concept adds 2 + 3 on a locally built curl 8.12.0.

**Limits in practice.** The result holds only under idealized unbounded resources. Real curl caps each expansion at 10,000,000 bytes, and each step is a nested config include using a C stack frame: a minimal countdown loop segfaulted after 922 steps on an 8 MB stack. A Bash loop re-invoking curl would add repetition, but that composes curl with Bash rather than showing curl alone is Turing-complete.

**History.** Variables arrived in 8.3 (2023-09), slicing in 8.12 (2025-02), and the cap in 8.17 (2025-11). The cap's pull request cited preventing self-inclusion loops, not Turing completeness. A reader later built "99 Bottles of Beer" with it.
