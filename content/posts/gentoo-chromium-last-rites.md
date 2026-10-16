---
title: "Gentoo masks Chromium after volunteers give up on maintaining it"
slug: "gentoo-chromium-last-rites"
date: 2026-10-16T08:02:29+0000
summary: "Gentoo's Chromium maintainers declared \"last rites\" on September 24, masking the package unless someone new takes it over before October 24. The trigger was a bug thread complaining that the stable ebuild was stuck on a version with 602 known vulnerabilities while newer builds were already in other distros. The maintainer called Chromium the package that burns through Gentoo maintainers the most: a complex build system, bundled dependencies, fast upstream churn, and a new Go dependency."
source: "https://lwn.net/Articles/1097760/"
source_title: "Last rites for Gentoo's Chromium package"
source_author: "Joe Brockmeier"
source_site: "LWN.net"
source_date: "2026-10-06"
hn_url: "https://news.ycombinator.com/item?id=50114359"
---

Gentoo's Chromium package is being retired. Maintainer Sam James declared "last rites" on September 24, which masks the ebuild so users don't install an unmaintained package. Anyone can still claim it before the period ends on October 24, but James said he doesn't want it restored without a plan for sustainable maintenance.

**What led to it**
- A user complained that the stable Chromium ebuild (151.x) carried 602 known vulnerabilities, while version 153 was already in Arch, openSUSE and Ubuntu. He had also complained in August.
- James replied that the maintainers are volunteers, that attached ebuilds from third-party overlays are useful but can't just be committed, and that every USE flag combination needs testing. Chromium takes 15 to 16 hours to compile on one commenter's laptop.
- He said the package is hard to maintain: complex build system, bundled dependencies, frequent upstream releases, and a new Go dependency. The thread's tone, ending with "This is ridiculous", finished it off.

**Open questions**
- A request to keep it at "best effort" was refused, since the volunteer-effort explanation had already been rejected.
- Chromium isn't offered as a binary package either, because of proprietary codec build problems.
- Users can keep running Chromium via overlays, but those are largely automated and have missed fixes found in Gentoo's version.
