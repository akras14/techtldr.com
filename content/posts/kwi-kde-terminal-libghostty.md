---
title: "Kwi, a KDE terminal on libghostty-vt, claims 870 fps vs Konsole's 23"
slug: "kwi-kde-terminal-libghostty"
date: 2026-11-01T15:02:48+0000
summary: "KDE developer Eike Hein has started Kwi, a new GPU-accelerated KDE terminal emulator built on Ghostty's libghostty-vt parsing library, Qt Quick/Kirigami and KDE Frameworks. He claims 3x to 35x better performance than Konsole depending on the benchmark (870 fps vs 23 fps in DOOM-fire-zig on one test system), plus fuller Kitty protocol support, better Wayland fractional scaling and improved screen-reader accessibility."
source: "https://blogs.kde.org/2026/11/01/introducing-kwi/"
source_title: "Introducing Kwi, a new KDE terminal emulator"
source_author: "Eike Hein"
source_site: "KDE Blogs"
source_date: "2026-11-01"
hn_url: "https://news.ycombinator.com/item?id=50324114"
---

**Bottom line:** Kwi is an early-stage, deliberately "unexciting" KDE Plasma terminal that pairs libghostty-vt (the headless terminal parsing and state library from Ghostty) with a Qt Scene Graph renderer using Vulkan or OpenGL. Konsole renders on the CPU through the older QWidget stack. Eike Hein says that gap accounts for the speedup he measured, and he notes the figures come from his own tests.

**What it offers over Konsole**
- A claimed 3x to 35x speedup depending on the benchmark. The headline example is 870 fps in DOOM-fire-zig vs 23 fps in Konsole.
- More complete, faster Kitty image support, plus the Kitty animation, mouse-cursor-shape and keyboard protocols, OSC 5522 and OSC 1337 clipboard, and OSC 9;4 progress.
- Better Wayland fractional scaling, ligatures, text decorations, grapheme widths (mode 2027), Nerd Font scaling and Powerline symbols.
- Better accessibility, including text extraction for screen readers, and live following of the Plasma color scheme.

**Why build it:** Konsole's embeddable KPart only works for QWidget apps (Dolphin, Kate, Yakuake), so Qt Quick/Kirigami apps have no KDE terminal component. Kwi ships a public QML module that its own application also uses, which the author says should also fix Konsole's split between its main settings dialog and the profile editor reachable from the KPart.

**Caveats:** The project is young and has fewer features than Konsole. libghostty-vt will only promise a stable v1.0 API/ABI in the next 4 to 6 months. The author also floats a longer-term idea of a KDE-based replacement for Kmscon, which he says has not been attempted yet.
