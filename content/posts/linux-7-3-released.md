---
title: "Linux 7.3 ships with a record 18,765 commits from 3,021 developers"
slug: "linux-7-3-released"
date: 2026-10-19T09:02:19+0000
summary: "Linus Torvalds released Linux 7.3 on October 18, with a record 18,765 non-merge commits from 3,021 developers, 807 of them first-time kernel contributors. Headline changes include binfmt_misc extensions, new group-scheduling work for large systems, the completed sched_ext subscheduler feature, the fchroot() system call and global per-CPU data for BPF programs. The 7.4 merge window opens now."
source: "https://lwn.net/Articles/1101143/"
source_title: "The 7.3 kernel has been released"
source_author: "Jonathan Corbet"
source_site: "LWN.net"
source_date: "2026-10-18"
hn_url: "https://news.ycombinator.com/item?id=50149392"
---

**Bottom line:** Linux 7.3 is out and is a record-breaker on every measure LWN tracks: 18,765 non-merge commits from 3,021 developers, 807 of whom were new to the kernel.

- **Release timing:** Torvalds shipped on schedule on Sunday, noting nothing special happened in the last week. The 7.4 merge window opens Monday, though he's on vacation and expects an unusual merging schedule for a while.
- **Notable changes:**
  - Significant binfmt_misc extensions.
  - Group-scheduling work aimed at performance on large systems.
  - A new `failfs` filesystem that fails all operations.
  - The `fchroot()` system call.
  - Global per-CPU data for BPF programs.
  - Completion of the sched_ext subscheduler feature.
  - Working-set tracking for virtual machines.
  - Alternative data-stream support in ntfs3.
  - A long list of SMB server improvements.
- **More detail:** LWN points to its two merge-window summaries and the KernelNewbies 7.3 page.
