---
title: "Linux 7.4 memblock binary search cuts KHO memory retrieval from 268s to 50ms"
slug: "linux-memblock-binary-search-kho"
date: 2026-10-25T05:02:39+0000
summary: "A Linux 7.4 memblock patch replaces a linear region search with a binary search in memblock_add_range() and memblock_isolate_range(), cutting memory retrieval time under Kernel Handover (KHO) from about 268 seconds to about 50 milliseconds."
source: "https://www.phoronix.com/news/Linux-7.4-memblock"
source_title: "Linux's Early Boot-Time Physical Memory Allocator Sees Big Speed-Up: ~268s To ~50ms"
source_author: "Michael Larabel"
source_site: "Phoronix"
source_date: "2026-10-24"
hn_url: "https://news.ycombinator.com/item?id=50223559"
---

**Bottom line:** The kernel's early boot-time physical memory allocator (memblock, `mm/memblock.c`) was using a linear search to find candidate regions. A Linux 7.4 pull request switches it to binary search, and in the worst case that drops KHO memory retrieval from roughly 268s to roughly 50ms.

- Google engineer Tarun Sahu spotted the linear scan in `memblock_add_range()` and `memblock_isolate_range()`.
- The slowdown shows up under heavy fragmentation, such as Kernel Handover (KHO, used by the Live Update Orchestrator) with many disjoint folios.
- Worst-case complexity becomes O(N log N), and O(N) for sequential appends.
- The patch is part of the memblock pull request sent for Linux 7.4. The article notes it only affects the early-boot allocator, which runs before the full page allocator is up.
