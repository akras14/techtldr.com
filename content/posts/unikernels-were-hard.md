---
title: "Unikernels Were Hard. Key Word: Were."
slug: "unikernels-were-hard"
date: 2026-10-10T20:02:47+0000
summary: "Geoffrey Huntley argues that AI coding agents remove the main obstacles to unikernels, such as missing libraries and drivers, because porting software is now a loop against a working original. He sees unikernels, with no shell or userland to exploit, as a far smaller attack surface than hardening Linux."
source: "https://ghuntley.com/unikernels/"
source_title: "unikernels were hard. key word: were."
source_author: "Geoffrey Huntley"
source_site: "ghuntley.com"
---
A unikernel makes the application the operating system: no userland, nothing to fork or spawn, and everything (web server, DNS, mail) is a library inside the app. That was the historic friction. Huntley and Justin Cormack, who worked on MirageOS, recall that early Mirage had TCP and HTTPS stacks but almost nothing for storage.

Huntley's claim is that the difficulty now lives in the past, since agents can port what is missing. If you have an original to compare against, porting is a loop: generate outputs from both implementations, diff them, port the tests. Cormack had an agent write mkfs.xfs in Rust with byte-identical output in a few hours. A missing Stripe library in OCaml can be ported from Go the same way. For storage, he suggests S3 as primary with a local NVMe cache.

On security, he calls the multi-user OS design debt from the era of human operators. A popped app on Linux gives an attacker a shell, and the model weights know what to do with one. With no shell or interpreter, a drive-by exploit becomes a targeted attack needing your source. Cormack fairly pushed back that attack-surface reduction is fuzzy: interpreters, memory-safety gadgets and writable-filesystem-free execution still exist. Huntley counters that the industry keeps trimming attack surface (build containers, Chainguard) rather than removing it.

He also praises NixOS machine tests and says upstream problems can be fixed with Nix overlays instead of waiting on maintainers. His conclusion: for satellite-grade needs use seL4; everyone else should seriously consider unikernels instead of hardening something hackable.
