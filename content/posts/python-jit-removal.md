---
title: "CPython's experimental JIT will be removed after PEP 836 was withdrawn"
slug: "python-jit-removal"
date: 2026-10-27T13:10:00+0000
summary: "CPython's experimental JIT, added for Python 3.13, is likely to be removed from the main branch early in the 3.16 cycle. The steering council paused JIT work in June and asked for a standards-track PEP justifying it. The authors wrote PEP 836, then withdrew it on October 19, saying they lacked the bandwidth to carry it forward."
source: "https://lwn.net/SubscriberLink/1102102/0c00e4d550f25f8c/"
source_title: "Python's experimental JIT to be removed"
source_author: "Joe Brockmeier"
source_site: "LWN.net"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50257419"
---

CPython's experimental JIT compiler, merged in early 2024 for Python 3.13, is on its way out. With its PEP withdrawn, the code is expected to be removed from main early in the Python 3.16 development cycle.

**How it got here**
- Brandt Bucher merged the JIT without a PEP; PEP 744 was written afterwards and was only informational, so it was never formal council approval.
- In June 2026 the steering council paused JIT work on main and asked for a standards-track PEP making the case for it, with a six-month deadline. Otherwise the code would be removed.
- PEP 836 (Bucher, Savannah Ostrowski, Ken Jin) appeared on July 3.
- On September 21 the council said the PEP answered its questions, though some answers were less convincing. It was judging whether the JIT should stay an experiment in main, not whether to make it a supported feature.

**The council's concerns**
- Memory impact and security, which it called "an underserved area".
- Maintainability and parity across platforms.
- User-visible behavior differences it considered "overly optimistic".
- A compatibility review scheduled "way, way too late".
- Design decisions made without wider review by core developers, and interactions with debugging, profiling and external JITs such as CinderX, Numba and PyTorch's compile.

**Withdrawal**
Ostrowski withdrew PEP 836 on October 19. The authors said they lacked the personal bandwidth to carry the current version forward. They still believe in the technical direction and plan to publish the reference implementation. Brett Cannon said work could continue outside the main repository, but a rival PEP is unlikely because everyone driving the JIT worked on PEP 836. Whether the original developers will continue is unclear.
