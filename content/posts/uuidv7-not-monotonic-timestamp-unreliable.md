---
title: "UUIDv7 only guarantees uniqueness: its timestamp and ordering can both break"
slug: "uuidv7-not-monotonic-timestamp-unreliable"
date: 2026-10-23T18:03:01+0000
summary: "Robert Alexander argues that RFC 9562 UUIDv7 does not strictly provide the properties people credit it with. The embedded timestamp may not match creation time, IDs are not strictly monotonic, and Python's uuid.uuid7() can race ahead of the clock after a backwards clock jump. Only collision resistance is solid."
source: "https://alexsci.com/blog/uuidv7-is-weird/"
source_title: "UUIDv7 doesn't have the properties you think it does"
source_author: "Robert Alexander"
source_site: "alexsci.com"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50211746"
---

**Bottom line:** The commonly cited UUIDv7 benefits (monotonic IDs, a useful creation timestamp, unguessability) are not strictly guaranteed, and RFC-compliant implementations can violate them badly. The only property you can rely on is uniqueness.

- **Timestamp:** the RFC lets implementations alter the timestamp and sets no bound on its distance from real time. Postgres warns that the extracted timestamp is not necessarily the generation time.
- **Runaway example:** in Python 3.14's uuid.uuid7(), after the clock is set back an hour, the embedded timestamp kept pulling further ahead of system time (about 3695s, then 3795s, ...) under load above one ID per millisecond.
- **Ordering:** cross-node monotonic ordering is impossible without synchronized clocks. Even on one node, an implementation with millisecond timestamps and no counter yields out-of-order IDs, and none of the implementations he checked persist the last timestamp across restarts.
- **Leakage:** the ID discloses a high-precision creation time, which can matter if an account's age or similar is meant to be private.
- **Advice:** roughly time-ordered IDs are still useful, so don't rip UUIDv7 out. Check what your implementation actually guarantees and what clock assumptions it makes.
