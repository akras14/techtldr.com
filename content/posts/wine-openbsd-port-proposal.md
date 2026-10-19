---
title: "Wine port for OpenBSD runs 64-bit Windows apps, using emulated %gs"
slug: "wine-openbsd-port-proposal"
date: 2026-10-19T15:02:51+0000
summary: "A draft emulators/wine port for OpenBSD was sent to the ports list. It runs dozens of 64-bit Windows programs, including the Witcher 3 remaster (with vkd3d-proton). 32-bit software doesn't work because WoW64 is unsupported."
source: "https://marc.info/?l=openbsd-ports&m=179236125692175"
source_title: "emulators/wine"
source_author: "Thomas Frohwein"
source_site: "openbsd-ports mailing list"
source_date: "2026-10-18"
hn_url: "https://news.ycombinator.com/item?id=50152307"
---

A draft `emulators/wine` port for OpenBSD was posted to the ports list, with the author calling it possibly "the most surprising port proposal of the year". Dozens of 64-bit Windows programs have been tested, mostly successfully, and the Witcher 3 remaster reportedly runs well once vkd3d-proton is added.

**What unblocked it.** OpenBSD can't set the `gsbase` register, which Windows code relies on. Andrii's workaround emulates the `%gs` prefix in software, with Fabien also credited for the heavy lifting.

**Limits.**
- WoW64 doesn't work, so 32-bit Windows programs won't run. `file(1)` shows whether an .exe or .dll is 32-bit.
- Wine's built-in tests are disabled. They need an X server and several still hang.

**Caution.** The author suggests a separate unprivileged user for testing, since Wine runs Windows binaries, malware included. Fencing it in with `unveil(2)` is still to be explored. Notepad++ is the suggested first test.
