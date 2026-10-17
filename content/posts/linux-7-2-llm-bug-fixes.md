---
title: "Linux 7.2: 4,830 Fixes-tagged commits as LLM bug-hunting takes off"
slug: "linux-7-2-llm-bug-fixes"
date: 2026-10-17T18:03:00+0000
summary: "LWN's stats for Linux 7.2 (released August 17) show a record 2,652 contributors and 16,418 non-merge commits, the second-busiest release ever. The notable shift is in bug fixing: 4,830 commits carry Fixes tags, a sharp rise after years of a flat trend, which Corbet attributes to people pointing LLMs at existing code. Commits with Assisted-by tags jumped from 301 in 7.1 to 1,111, and the true count is likely much higher. Corbet questions whether a pile of code nobody fully understands is being created."
source: "https://lwn.net/Articles/1088776/"
source_title: "Development statistics for the 7.2 kernel"
source_author: "Jonathan Corbet"
source_site: "LWN.net"
source_date: "2026-08-17"
hn_url: "https://news.ycombinator.com/item?id=50132950"
---

**Bottom line:** Linux 7.2 was one of the busiest cycles on record, and LLMs are visibly changing it, both in bug finding and in who is submitting patches.

- **Volume:** 16,418 non-merge commits from 2,652 developers (a record; 7.1 had 2,479). Only 6.7 had more commits, and that was inflated by the bcachefs history merge. A record 613 contributors were new.
- **Bug fixes jump:** 4,830 commits carry `Fixes` tags. After years of a roughly level line, the number rises sharply, which Corbet reads as the effect of many people aiming LLMs at existing code. The crossover where the kernel started fixing more bugs than it introduced was 5.9, but that line will shift as post-5.9 bugs are found.
- **LLM-assisted patches:** 1,111 changes (under 7%) carry `Assisted-by` tags, up from 301 in 7.1. The real number is probably far higher, since many developers don't add tags and at least one maintainer strips them. About 700 commits reference the Sashiko LLM review system (252 with `Reported-by` credit).
- **Review signal:** `Reviewed-by` tags (48% of commits) show a mild downward trend; whether real review is declining is unclear.
- **Concern:** Corbet notes LLMs let people write kernel patches they couldn't before, and that submitters may not fully understand them. The risk is a pile of code nobody has understood that must still be maintained. The kernel is production software, and this is a large experiment on its development process. About 14,000 changesets, 1,100+ with `Assisted-by`, are queued for 7.3.
- Note: the HN title ("bugs fixed per release over time: >2× rise") is an editorialization; the article itself is cautious.
