---
title: "Ghostty 1.4 fixes 10 security bugs and compresses scrollback 70-90%"
slug: "ghostty-1-4-kitty-graphics-security"
date: 2026-10-27T18:02:58+0000
summary: "Ghostty 1.4.0 (7 months, 172 contributors, 2,889 commits) ships fixes for ten security advisories, scrollback compression that cuts memory use of off-screen history by 70% to 90%, and a complete Kitty graphics implementation, the first outside Kitty itself. If you run Ghostty, update."
source: "https://ghostty.org/docs/install/release-notes/1-4-0"
source_title: "Ghostty 1.4.0 Release Notes"
source_site: "ghostty.org"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50261998"
---

**Bottom line:** Ghostty 1.4.0 is a large release, and the security section is the part to read first. It lists ten advisories, mostly triggered by displaying malicious terminal output or by a user paste, click or drag-and-drop.

## Security
- Paste protection did not treat carriage returns as line endings, so a paste could run a command unconfirmed in programs without bracketed paste.
- Malicious Kitty graphics commands could crash or hang the terminal, force huge allocations, or in limited cases read or delete files outside the allowed temp directories.
- Malicious output via tmux control mode could crash Ghostty and close every window (1.3.0 and 1.3.1; tmux need not be installed). A second advisory covers a zero-sized or oversized pane layout.
- A crafted directory name could make the zsh shell integration emit arbitrary escape sequences, such as silently replacing the clipboard via OSC 52. Bash was affected in a more limited way. Just `cd`-ing into an untrusted cloned repository is enough.
- On macOS: cmd-clicking crafted OSC 8 hyperlinks (and links found by the default URL matcher) could lead to command execution, drag-and-drop text skipped paste protection, and file names with control characters could run a different command than the confirmation dialog showed.

The maintainer notes that CVEs were requested weeks ago but not yet assigned.

## Features and performance
- **Scrollback compression:** non-visible scrollback is compressed while the terminal is idle and decompressed on demand, cutting its memory by 70% to 90% for typical text. The default scrollback limit rises from 10 MB to 50 MB per terminal.
- **Kitty graphics:** animation and relative placements are now supported, so the implementation covers 100% of the spec. Image payloads parse about 16x faster using SIMD (64 MiB benchmark, x86_64 Linux).
- **macOS 27 support**, while staying compatible back to macOS 13.
- Improved SSH integration and hundreds of other fixes.
