---
title: "A 37-year-old 16-bit counter in OpenBSD became a local root exploit"
slug: "openbsd-shm-nattch-overflow-root"
date: 2026-10-16T16:03:00+0000
summary: "Calif reports a local root exploit for OpenBSD that works with KARL, SMAP and SMEP enabled and needs no info leak. It starts from a bug dating to the original BSD sources: the shared-memory attach counter is a signed 16-bit short, and forking about 259 processes against 127 attached segments wraps it negative, which enables a use-after-free race. Chained with a separate out-of-bounds write, it corrupts a kernel pointer at a predictable address. Both issues are fixed in errata 026 and 018. Calif argues for safe-unlinking, a randomized self-referential PTE slot and randomized kernel allocator bases to kill the technique."
source: "https://calif.io/research/big-fish"
source_title: "Big Fish: Exploiting a 37-Year-Old Kernel Bug in OpenBSD"
source_author: "Andrew Griffiths"
source_site: "Calif"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50118977"
---

**Bottom line:** Calif found that OpenBSD's SysV shared-memory attach count (`shm_nattch`) is a signed 16-bit short, a definition inherited from the original BSD sources about 37 years ago. Overflowing it led to local root even with KARL, SMAP and SMEP on, without needing an information leak. The bugs were reported and fixed in errata 026 and 018.

## The exploit path

- Map 127 shared-memory segments, unmap them, and fork 259 processes so the attach count wraps to a negative value (-32,643).
- Groom the kernel heap, using `sysctl(KERN_MALLOCSTATS)` counters to see when new pages are allocated.
- Win a race between `IPC_RMID` in one process and attaching a size-confused segment in a helper, giving a use-after-free.
- Use a separate conditional out-of-bounds write (a `-1`) to overwrite the membership counters of a 16-byte `struct ip_moptions`.
- Closing the socket makes the kernel walk the bogus group list, and the resulting atomic decrements flip the self-referential page-table mapping from user to supervisor accessible. That yields kernel code execution by overwriting a pointer at a predictable address, after which the exploit restores the pointer and returns a root shell.

Calif frames the primitive as write-what-where: a `TAILQ_REMOVE` is the write, the attacker's own userland mapping is the "what", and a predictable kernel function pointer is the "where".

## Mitigations Calif proposes

- **Safe-unlinking:** check doubly linked list invariants before unlinking, as FreeBSD and NetBSD already do.
- **Randomize the recursive PTE map:** pick a PML4 slot at boot from a random byte, so the page-table base is no longer fixed.
- **Randomize the kernel allocators:** KARL randomizes the kernel binary, but the boot-time and best-fit allocators are deterministic, so early structures with function pointers land at the same addresses every boot. Randomizing base addresses is nearly free; randomizing allocation order at run time costs some overhead.
- **Compiler features:** the array bounds sanitizer in release builds (Calif privately reported 21 FreeBSD issues it would have caught) and upcoming Alloc Tokens to separate pointer-bearing allocations from plain data.

Calif says that as AI-assisted vulnerability discovery spreads, eliminating whole exploit classes matters more than patching individual bugs. A proof of concept is linked from the post.
