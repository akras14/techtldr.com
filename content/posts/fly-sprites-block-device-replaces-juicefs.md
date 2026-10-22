---
title: "Fly.io replaces JuiceFS under Sprites with a block device backed by a bucket"
slug: "fly-sprites-block-device-replaces-juicefs"
date: 2026-10-22T16:04:00+0000
summary: "Fly.io says its Sprites VMs now run on SBD, a block device served over NBD from object storage, with ext4 on top, replacing a modded JuiceFS plus SQLite setup. In a stress test of 400 JuiceFS Sprites over about 5,000 write/checkpoint/sleep/wake/restore cycles, some permanently lost the ability to checkpoint. Fly.io says SBD is orders of magnitude more reliable, though it gives no hard numbers."
source: "https://fly.io/blog/sprites-block-device/"
source_title: "SBD: We Have a Filesystem at Home"
source_author: "Daniel Botha"
source_site: "fly.io"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50194824"
---

**Bottom line:** Fly.io has replaced the filesystem under Sprites, its agent-oriented VMs, with the Sprites Block Device (SBD). SBD exposes an object-storage bucket to the Linux kernel as a block device over NBD, formatted as ext4. It replaces a heavily modified JuiceFS with a SQLite metadata store kept durable by Litestream. Pre-10/6 Sprites can be migrated.

## What went wrong with the old design

A Sprite's disk was chunks in a bucket, a metadata database mapping chunks to a disk image, and a second database (`checkpoints.db`) mapping checkpoints to images. Each was made durable separately, so they could disagree. Running JuiceFS in writeback mode recorded metadata before uploading the bytes, so a machine killed in between left metadata pointing at a missing chunk. ext4 then hit a failed checksum and went read-only. Checkpoints were copy-on-write clones sharing the same chunks, so restoring one didn't help.

Fly.io built a harness that drove fleets of Sprites through write, checkpoint, sleep, wake and restore while verifying every byte. Against 400 JuiceFS Sprites over about 5,000 cycles, some permanently lost the ability to checkpoint.

## How SBD works

- **One daemon, one bucket.** `sbd serve` answers kernel block reads and writes from a local cache, uploads dirty blocks in the background, and flushes on demand. Metadata lives in the bucket next to the blocks.
- **Simpler checkpoints.** A checkpoint is a versioned block snapshot of the whole volume in SBD's own catalog. Restore unmounts, rewinds the volume in place and remounts, at about the cost of a restart.
- **Fail loudly.** SBD verifies every block it reads back against what it wrote. If SBD dies, the Sprite restarts rather than continue on a disk it can't vouch for.
