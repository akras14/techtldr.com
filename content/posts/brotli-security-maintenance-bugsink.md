---
title: "Brotli's Google-maintained C library sat unreleased for eight months after an out-of-bounds fix"
slug: "brotli-security-maintenance-bugsink"
date: 2026-10-25T20:05:00+0000
summary: "Bugsink's author argues Brotli is too poorly maintained to trust with untrusted input. The decoder memory-corruption bug behind the Node.js RCE was fixed in the repo on February 27, 2026 but only released on October 21; a 2025 decompression-bomb fix likewise sat unreleased for 283 days and shipped with no documentation on how to use it. Three out-of-bounds reads are open as pull requests, and releases happen whenever the single maintainer has time. His advice: add an option to turn Brotli decoding off, and ask whether you need to support it at all."
source: "https://www.bugsink.com/blog/bye-bye-brotli/"
source_title: "Bye Bye Brotli?"
source_author: "Klaas van Schelven"
source_site: "Bugsink"
source_date: "2026-10-25"
hn_url: "https://news.ycombinator.com/item?id=50230362"
---

**Bottom line:** Klaas van Schelven, who builds the Bugsink error tracker, says it's time to stop trusting Google's Brotli package with untrusted input. Its security fixes land in the repo long before they reach a release, and the project is effectively run by one maintainer in spare time.

**The latest incident.** The memory-corruption bug in the Brotli decoder (the one behind the Node.js "HTTP2Shell"/"Node2Shell" RCE, see our [earlier post](/nodejs-brotli-decoder-rce-patch/)) was fixed upstream on February 27, 2026. Google apparently didn't treat an attacker-controlled read/write past the end of an eight-byte buffer as a security issue, so no release followed until October 21. Node published its advisory before the Brotli release, leaving everyone else to work out the impact.

**The previous incident.** In 2025, Brotli's decompression-bomb mitigation (a new `output_buffer_limit` argument for the Python decoder) was committed publicly in September 2024, merged in January 2025, and noted as SECURITY in the release notes, yet the C library wasn't tagged until October 27, 2025, 405 days after the first commit. The new parameter was never documented. urllib3 implemented a limit anyway, and 153 days later had to fix that implementation because it introduced another high-severity decompression-bomb bug. Bugsink, which decodes `Content-Encoding: br` requests, was exposed to trivial memory-exhaustion DoS in the meantime.

**Current state.** The author points to three open pull requests for out-of-bounds reads, open complaints about missing release notes and release timing, and a maintainer saying releases happen when he can find a slice of time. He expects more severe bugs.

**What he's doing.** Adding a switch to disable Brotli decoding for self-hosters, asking upstream Sentry (whose Python SDK sends Brotli-encoded payloads) whether they can drop it, and keeping a Python wrapper around a Rust Brotli implementation as an escape hatch. His advice to others: consider whether you need Brotli support at all.

This is one maintainer's opinionated take; the timeline claims come from his reading of the repository history.
