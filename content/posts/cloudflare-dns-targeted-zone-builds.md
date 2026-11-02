---
title: "Cloudflare cut DNS zone-build memory 85% by rebuilding only what changed"
slug: "cloudflare-dns-targeted-zone-builds"
date: 2026-11-02T17:34:59+0000
summary: "Cloudflare changed its DNS Zone Builder from rebuilding an entire zone on every change to building only the records and metadata an event actually touches. In a 24-hour side-by-side run, 92.95% of builds avoided a full-zone rebuild, aggregate memory fell from 49 GiB to 7 GiB (peaks 79 to 12 GiB), the worst p99 delay dropped from 14.6 minutes to 1.21 minutes, and median change-to-Quicksilver delay halved from about 7.7s to 3.3s."
source: "https://blog.cloudflare.com/dns-targeted-zone-builds/"
source_title: "How we halved DNS build delay and cut memory use by 85%"
source_site: "Cloudflare Blog"
source_date: "2026-11-02"
hn_url: "https://news.ycombinator.com/item?id=50337558"
---

**Bottom line:** Cloudflare's Zone Builder used to rebuild every record of a DNS zone whenever anything changed, which stopped scaling for zones with tens of millions of records. It now plans the smallest build that is provably correct, and falls back to a full build whenever it can't prove that. The rollout cut memory by about 85% and halved the time from a DNS change to its data landing in Quicksilver (Cloudflare's distributed key-value store).

## What changed

- **Events say what changed.** The producer now includes changed fields and settings, with old and new values where available. Timestamp-only updates, unknown events and missing details still trigger a full build: the planner never turns uncertainty into less work.
- **Five build types:** resources only (ordinary A/AAAA/TXT/CNAME edits), resources plus metadata (nameserver and wildcard changes), apex plus metadata (e.g. Universal SSL, which can alter CAA records), purge, and full zone. The rule is that every key a targeted build writes must be byte-for-byte what a full build would write.
- **Shared metadata.** One metadata key is shared by every Cloudflare zone with the same domain name. Targeted builds re-read the current same-name zone list just before executing and rebuild that key from the fresh view, writing a zone's records before the metadata that refers to them.
- **Plan twice.** An event produces a conservative work list; just before execution a fresh snapshot can upgrade it to a full build, turn it into a purge, or skip it.
- **One unified scheduler** replaces separate record and full-zone schedulers. It merges queued events for the same zone into a single build, and keeps Kafka messages attached to the work until the result is safely written. Kafka offsets are committed only after all earlier messages finish ("soft committing"), so a restart replays work rather than losing a change.
- **Very large zones** above a record-count threshold never escalate to a full build, since it can't fit in memory and would block its Kafka partition. They rebuild the apex, metadata and the named records instead.

## How they verified it

Equivalence tests compare a full and a targeted build from the same database state. A mirror Zone Builder consumed the production Kafka stream and wrote to a separate Quicksilver; a comparer checked a 20% sample of zones for missing, extra or different keys (0.31% diff rate across 234,313 matched zones, mostly timing). The rollout then went by hash of zone name, free zones first and then paid.

## Results (24 hours)

- Build mix: 86.81% resources only, 7.04% full, 2.54% resources and metadata, 2.30% apex and metadata, 1.30% purge. Full builds fell by about 40%, and the remaining ones average 0.35s because they mostly involve new, small zones.
- Kafka consumption now tracks production (about 600 to 800 messages/s) instead of bursting up to 1,550/s while catching up.
- Largest p99 build-delay spike fell from 14.6 to 1.21 minutes. Typical delays barely moved.
- Memory: average 49 to 7 GiB, peak 79 to 12 GiB.
- With builds cheaper, Cloudflare also shortened the per-zone rebuild interval and the Kafka aggregator's hold time, halving the median delay from about 7.7s to 3.3s.

Cloudflare notes it no longer plans to eliminate full builds entirely, as a 2022 post had suggested, since they are sometimes necessary and cheap for small zones.
