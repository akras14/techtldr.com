---
title: "Linux 7.4 speeds up FAT32 file preallocation by up to 265x"
slug: "fat32-fallocate-265x-linux-7-4"
date: 2026-10-31T04:02:18+0000
summary: "A few-line patch merged for Linux 7.4 makes the FAT driver allocate multiple clusters in one call instead of one at a time when a file grows via fallocate with FALLOC_FL_KEEP_SIZE, giving up to a 265x speedup in the author's benchmarks."
source: "https://www.phoronix.com/news/Linux-7.4-Faster-FAT32"
source_title: "FAT32 File-System Seeing As Much As 265x Improvement With Linux 7.4"
source_author: "Michael Larabel"
source_site: "Phoronix"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50310689"
---

A patch of just a few lines, merged for Linux 7.4, speeds up FAT32 preallocation by as much as 265x in benchmarks shared by its author, Matheus Alves de Almeida.

- **The cause:** when a file grew through `fallocate` with `FALLOC_FL_KEEP_SIZE` (reserve disk space without changing the reported file size), the FAT driver did many separate one-cluster allocations.
- **The fix:** allocate multiple clusters in a single call.
- **Status:** merged this week as part of Andrew Morton's non-MM pull request for Linux 7.4.
