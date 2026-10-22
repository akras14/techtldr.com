---
title: "Six unpatched Linux kernel bugs, two at CVSS 9.0, leaked to the public mailing list"
slug: "linux-kernel-zdi-vulns-posted-to-public-list"
date: 2026-10-22T12:04:46+0000
summary: "Trend Micro's Zero Day Initiative apparently sent several vulnerability reports to the public Linux kernel mailing list instead of the private security list. Two are CVSS 9.0 remote code execution bugs in the SMB server code, and none are fixed in mainline yet."
source: "https://www.phoronix.com/news/LKML-New-Trend-Vulnerabilities"
source_title: "Several Linux Kernel Vulnerabilities Accidentally Posted To Public Kernel Mailing List"
source_author: "Michael Larabel"
source_site: "Phoronix"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50193804"
---

Trend Micro's Zero Day Initiative apparently sent several vulnerability reports to the public Linux kernel mailing list instead of the private security list. Two are CVSS 9.0 remote code execution bugs in the SMB server code, and none are fixed in mainline yet. Greg Kroah-Hartman replied to the reports asking why they were posted publicly.

The disclosed issues:

- **ZDI-CAN-33549** (CVSS 9.0): SMB Direct remote code execution from an integer underflow. A patch sits in a branch aimed at Linux 7.5 and will likely be expedited.
- **ZDI-CAN-33774** (CVSS 9.0): a KSMBD race condition leading to remote code execution.
- **ZDI-CAN-33367**: `skb_zerocopy` in the network code, local privilege escalation. Kernel developers got a separate report of the same bug last week, and a patch is expected soon.
- **ZDI-CAN-34068**: VMWGFX (VMware graphics) out-of-bounds write, local privilege escalation.
- **ZDI-CAN-34076**: uninitialized resource in NVMET, information disclosure.
- **ZDI-CAN-32005**: search-domain TCP query stack buffer overflow, local privilege escalation. This one was posted a day earlier.

Some have patches in testing or queued in `-next` branches. If you run ksmbd, expose SMB Direct, or use these drivers, watch for stable kernel updates.
