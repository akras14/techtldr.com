---
title: "Claude wrote a Linux scanner driver in hours by diffing USB traces against Windows"
slug: "reverse-engineer-hardware-claude-harness"
date: 2026-10-15T16:02:41+0000
summary: "Christian Schaller used Claude Code with a KVM Windows VM and host-side USB captures to get a working Linux driver for a Plustek OpticFilm 8200i film scanner within hours, after months of being stuck, and then to patch his laptop's MIPI camera stack. He has published the generic harness as hw-harness."
source: "https://blogs.gnome.org/uraeus/2026/10/15/aiandreverseengineerhardware/"
source_title: "Using Artificial Intelligence to reverse engineer hardware"
source_author: "Christian F.K. Schaller"
source_site: "blogs.gnome.org"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50104299"
---

Schaller's scanner used a Genesys GL128 chip with no Linux support. He had given up for months, then retried after seeing someone describe giving an AI agent full control of a Windows instance to capture driver data. With a Windows VM he could scan in, plus captured USB traffic, Claude produced a first working scan within hours and a near-complete driver (including infrared) days later. Upstream SANE had meanwhile added support for the same scanner, so the result is mostly a proof of concept.

His approach, now packaged as **hw-harness** for Claude or Codex sessions:

1. Run the vendor's Windows driver in a KVM VM with the real device passed through.
2. Capture bus traffic on the Linux host with usbmon, below the passthrough layer.
3. Repeat the operation with your own Linux driver and capture it the same way.
4. Diff the two traces; the differences are the driver bugs.

For devices with no bus to tap, such as an internal MIPI camera, it inspects Windows-side INF files, registry data, ACPI tables and device properties. He says nothing is copied from vendor code; every constant traces to something recorded. The same harness gave him kernel and libcamera patches for his laptop camera in one day.

**Results (single unit, single frame, so one data point):** his backend scans at full resolution in 34.6 s versus 54.4 s for upstream and adds infrared dust removal; upstream offers more resolution steps. He also built Negative Vibes, a Rust GNOME app for scanning negatives.

His argument: this kind of work (reading USB captures, spotting repeated sequences, proposing and testing what a byte means) suits an AI model, while the human still decides what to build and verifies on real hardware.
