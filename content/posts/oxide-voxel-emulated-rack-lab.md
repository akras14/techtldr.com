---
title: "Oxide's Voxel emulates a full rack on a mini-PC with stock software"
slug: "oxide-voxel-emulated-rack-lab"
date: 2026-10-14T22:03:04+0000
summary: "Oxide built Voxel, a software emulation of a whole Oxide rack on illumos, so engineers can test without scarce hardware. It runs the unmodified production control plane. The author runs six sleds on a mini-PC with a 32-thread Ryzen 9 and 96GB RAM, and a full rack comes up in about 10 minutes."
source: "https://oxide.computer/blog/voxel-labs"
source_title: "Virtual Oxide Labs: Emulating a Full Oxide Rack"
source_author: "Steve Karam"
source_site: "oxide.computer"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50092423"
---

Oxide's Product Assurance team built Voxel (Virtual OXidE Labs), a software emulation of a full Oxide rack. Real racks go to customers, so tests need a stand-in. Voxel's rule is fidelity: it runs stock Oxide software, with no hacks and no special test builds. It succeeds the older VM-based `a4x2` labs.

## What it emulates

- Sleds as Propolis VMs, on illumos, written in Rust.
- Switches as P4 switches via SoftNPU, a software P4 switch.
- BGP (numbered and unnumbered) or static networking, using Debian and FRR VMs.
- Service Processor and Root of Trust microcontroller simulation and emulation.

The author runs it at home on a Minisforum MS-A2 mini-PC (32-thread AMD Ryzen 9 9955HX, 96GB RAM) with Oxide Helios and a fast NVMe drive. That handles 6 sleds, as one rack or two racks under one management plane. A full rack is up about 10 minutes after launch.

## How it works

1. **Image creation.** `voxel image create` builds a sled VM, installs the control plane (Omicron) either from source or from a TUF release repo, and snapshots it as a ZFS snapshot just before configuration. That snapshot is reused to make every sled.
2. **Launch.** `voxel launch --emu` takes the snapshot, an FRR networking image and a `voxel-init` agent that gives each virtual sled its role. It starts the emulated SPs, creates the network links and boots the VMs.

Oxide also keeps real "racklettes" (four sleds, two switches) for hardware testing, four today and soon eight, plus simulated modes for running parts of Omicron.
