---
title: "Getting old Macromedia Director games to run on modern hardware"
slug: "macromedia-director-copy-protection"
date: 2026-10-10T13:02:49+0000
summary: "WerWolv got a 2000s German CD-ROM game (Findus bei den Mucklas) running again by defeating three layers of copy protection: decoy files on the disc image, a CD-presence check in the embedded Director script, and a copy-protection DLL. The fixes were a one-instruction bytecode patch and a stub in the DLL."
source: "https://werwolv.net/posts/macromedia_copy_protection/"
source_title: "Getting old Macromedia Director Games to run on modern Hardware"
source_author: "WerWolv"
source_site: "WerWolv"
---

**Bottom line:** WerWolv revived a childhood CD-ROM adventure game by working through three copy protection layers. The whole fix came down to a 3-byte patch in the game executable and a stubbed-out function in a support DLL. The same method should apply to other Macromedia Director games, though not with one common patch.

**The three layers:**

1. **Decoy files.** The disc image (a CloneCD .ccd/.cue/.img set, converted with ccd2iso) contains duplicate directory entries, where the real file is shadowed by 0- or 256-byte garbage copies. Different tools showed different combinations, so the author extracted the right files manually using several tools, then ran the installer under Wine in a separate prefix.
2. **CD check in the Director script.** The game is a Macromedia Director "projector" executable with a .dxr movie appended, which is why the error string didn't appear in Ghidra. Using ProjectorRays, a Director decompiler, plus a new `--offset` option the author contributed, they dumped the Lingo script. The script loops over drive letters looking for a CD-ROM with a `p3media` folder, and quits with an alert if it finds none. Changing one conditional jump (`95 00 60` to `95 00 03`) skips the quit branch, and the intro video played.
3. **Copy-X DLL.** Intro and house scenes call `initdisplay()` in `optgraph.dll`, which talks to the CD drive and must return the magic value 26079537. Rather than reverse the logic, the author used Ghidra to patch the function to just return that constant. The protection isn't enforced on Mac at all.

**Result and tooling.** The game reached its menu screen. The author wrote a Python auto-patcher that checks SHA-256 hashes and expected bytes before patching, written for the German release but adaptable to other versions. Many other games from the same period use the same basic scheme, so the process carries over with some per-game effort.
