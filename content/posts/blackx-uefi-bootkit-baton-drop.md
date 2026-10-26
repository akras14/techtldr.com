---
title: "BlackX, a BlackLotus-style UEFI bootkit, hit patched Windows PCs in Central Asia"
slug: "blackx-uefi-bootkit-baton-drop"
date: 2026-10-26T20:03:00+0000
summary: "Kaspersky found BlackX, a UEFI bootkit that closely resembles BlackLotus and exploits the same Secure Boot bypass, Baton Drop (CVE-2022-21894). It was used in early 2026 against diplomatic agencies in Tajikistan, Uzbekistan and Turkmenistan, and Kaspersky ties it to APT41 with medium confidence. The attackers had already compromised the networks and gained admin rights before installing it, so it serves as persistence rather than initial access."
source: "https://securelist.com/uefi-bootkit-blackx/121539/"
source_title: "New BlackX UEFI bootkit exploiting Baton Drop"
source_author: "Kaspersky GReAT"
source_site: "Securelist"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50249616"
---

**Bottom line:** Kaspersky's GReAT team found a new UEFI bootkit, BlackX, that is very similar to the 2023 BlackLotus bootkit and uses the same Secure Boot bypass, Baton Drop (CVE-2022-21894). It was used in early 2026 against diplomatic agencies in Tajikistan, Uzbekistan and Turkmenistan.

- **Delivery:** attackers first compromised the network and got administrator rights, then dropped an installer (`inst.exe`) plus an RC4-encrypted file holding everything else. The installer binary itself contains no malicious strings, which makes it harder for responders to recover the components.
- **Exploit:** the chain is "remarkably similar" to BlackLotus, with differences, and aims to enroll attacker Machine Owner Keys so the firmware loads arbitrary EFI files. BlackLotus source was never published, so Kaspersky suspects the authors reverse-engineered or bought it. It is unusual to see an APT group borrow from a criminal tool.
- **Kernel driver:** it disables security-product callbacks, tampers with ETW providers, and hides the bootkit's files by returning a handle to `calc.exe` when something opens them. Some tampering only targets Kaspersky and Avast, and did not work against Kaspersky in testing.
- **Payloads:** a .NET-loading RAT that takes commands through Google Drive, OneDrive or Yandex.Disk, and a loader Kaspersky calls OxiDLLRunner.
- **Attribution:** medium confidence in APT41, partly based on the string "xdd" appearing in both BlackX and a path ESET reported for a related group.

Secondary write-ups say the machines were fully patched, because Microsoft's revocation of the old Windows Production PCA 2011 certificate (KB5025885) is not enabled by default. Check that against Kaspersky's report and Microsoft's guidance before acting.
