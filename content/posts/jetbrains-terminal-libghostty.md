---
title: "JetBrains IDE terminal replaces JediTerm with libghostty in 2026.3"
slug: "jetbrains-terminal-libghostty"
date: 2026-10-27T17:04:20+0000
summary: "Starting with 2026.3, the built-in terminal in JetBrains IDEs uses libghostty, the terminal library behind Ghostty, instead of JediTerm. It adds the Kitty keyboard protocol (so Shift+Enter works in agent CLIs like Claude Code and Codex), theme-aware dark/light queries, and about 50 control sequences JediTerm ignored or misread. It is on by default in the 2026.3 EAP, with a registry flag to switch back."
source: "https://blog.jetbrains.com/platform/2026/10/jetbrains-terminal-now-uses-ghostty/"
source_title: "The Terminal in JetBrains IDEs Now Runs on libghostty"
source_author: "Ekaterina Valeeva"
source_site: "The JetBrains Blog"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50260714"
---

JetBrains swapped its long-used terminal emulator, JediTerm, for libghostty, motivated by terminal apps such as AI coding agents, Neovim and tmux that rely on modern protocols JediTerm never supported. libghostty now handles parsing and terminal state; the IDE still does rendering and higher-level features.

- **Keyboard:** support for the Kitty keyboard protocol and xterm modifyOtherKeys lets apps tell Enter, Shift+Enter and Ctrl+Enter apart. Shift+Enter now starts a new line in agent prompts instead of sending a half-written message. The old Esc+CR workaround setting is ignored.
- **Theme:** the terminal answers dark/light queries and OSC 4 / OSC 21 palette queries from the IDE theme and notifies apps when the theme changes. Claude Code, OpenCode, Pi, Neovim and tmux can follow the IDE automatically.
- **Rendering:** nearly 50 control sequences that JediTerm ignored or misread now work. Examples: REP (missing characters in ncurses output), colon-separated SGR colors (wrong attributes), and APC/SOS/PM strings (previously printed as text).
- **Next:** libghostty already parses OSC 52 clipboard, desktop notifications, progress bars, Kitty graphics and styled underlines, but the IDE doesn't act on them yet. JetBrains expects those to arrive faster than with JediTerm.
- **Availability:** default in the 2026.3 EAP and expected to stay through stable. To revert, turn off the `terminal.use.ghostty.emulator` registry key; bugs go on IJPL-218849.
