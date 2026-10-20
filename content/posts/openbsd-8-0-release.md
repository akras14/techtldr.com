---
title: "OpenBSD 8.0 raises amd64 kernel address space from 4G to 512G"
slug: "openbsd-8-0-release"
date: 2026-10-20T04:03:00+0000
summary: "OpenBSD 8.0, the 61st release, shipped on October 20, 2026. Headline changes are a 512G amd64 kernel virtual address space (up from 4G) with a randomly placed direct map, hibernate on arm64, Intel Arc A-Series graphics support, and vmd(8) guests with up to 64 vCPUs. Support for the loongson architecture is retired."
source: "https://www.openbsd.org/80.html"
source_title: "OpenBSD 8.0"
source_author: "OpenBSD project"
source_site: "openbsd.org"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50160494"
---

**Bottom line:** OpenBSD 8.0 is out. The biggest low-level change is that the amd64 kernel virtual address space grows from 4G to 512G, which also lets the kernel map more than 4GB of GPU VRAM.

## Platforms

- **amd64:** the pmap(9) direct map now sits at a random location; swap encryption is faster using AES CPU instructions.
- **arm64:** hibernate support, bounce buffers, and the same AES swap speedup.
- **riscv64:** kernel support for the 'V' vector extension and the Sstc timer, the Advanced Interrupt Architecture (imsic(4), aplic(4) drivers), and CPU extension detection exposed via `elf_aux_info(3)`.
- **sparc64:** switched to the LLVM toolchain.
- **loongson:** support retired because it doesn't work with Clang 22.

## Graphics and power

- drm(4) updated to Linux 6.18.53, with support for Intel Arc A-Series (DG2/Alchemist) cards.
- Several suspend/resume fixes for Qualcomm-based laptops, plus new input validation in the unhibernate path against integer overflow and disk image overreads.

## Virtualization

- vmd(8) guests can have up to 64 vCPUs and use x2apic, checksum offload and MSI/MSI-X virtio interrupts. VM ownership is now managed with files.
- Fixes prevent out-of-bounds reads in vmd(8)'s ELF loaders when given malformed files.

## Userland

- New `getexecpath(3)`, DUID support in `mount(8) -u`, and more `sndioctl(1)` controls.
- Assorted bugfixes, including an infinite loop in `patch(1)` on malformed ed(1) scripts.
