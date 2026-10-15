---
title: "Libreboot 26.10 adds 233 Chromebooks, defaults to EDK2 and bans AI contributions"
slug: "libreboot-26-10-chromebooks-edk2"
date: 2026-10-15T14:06:00+0000
summary: "Libreboot 26.10 imports all 233 Chromebooks supported by MrChromebox's coreboot distro (mostly source-only), switches the default payload to EDK2 UEFI, and adds a strongly anti-AI policy in AGENTS.md. The maintainer warns the release makes major design changes that may affect existing setups."
source: "https://libreboot.org/news/libreboot2610.html"
source_title: "Libreboot 26.10 \"Ominous Olive\" released! With full UEFI support."
source_author: "Leah Rowe"
source_site: "Libreboot"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50102923"
---

Libreboot 26.10 imports all 233 Chromebooks supported by MrChromebox's coreboot distro (mostly source-only), switches the default payload to EDK2 UEFI, and adds a strongly anti-AI policy in AGENTS.md. The maintainer warns the release makes major design changes that may affect existing setups, and says to read the changelog twice.

## What changed

- **Chromebooks:** 233 Intel and AMD boards were imported with a new `util/mrimport/` tool. Only Baytrail (Kip) is tested so far, and the rest are source-only. Libreboot sets the HAP bit at build time to disable Intel ME and unlocks flash descriptor regions. MrChromebox is treated as upstream, and his installation scripts should still work.
- **EDK2 by default:** GRUB and SeaBIOS now ship as EFI builds inside EDK2 (SeaBIOS via a CSM wrapper), reachable with ESC at boot. The bare-metal coreboot GRUB and SeaBIOS payloads are deprecated and may be removed. MemTest86+ and iPXE (on some boards) are included, and Windows is now said to be compatible.
- **AMD Fam15h boards (e.g. KGPE-D16):** these move to the 15h.org coreboot fork, which uses AMD AGESA. This gives more reliable RAM and CPU init and working IOMMU and TPM, and the maintainer says the boards "actually work for the first time". EDK2 images there are source-only.
- **AI policy:** the new AGENTS.md is called the release's most important change. The notes also say Libreboot considers U-Boot effectively dead on x86 after upstream disputes over AI-assisted code, though it keeps U-Boot for ARM64.
