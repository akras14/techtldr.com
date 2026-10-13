---
title: "Gentoo maintainer asks Python projects to avoid uv-build (1.5 MiB Rust backend)"
slug: "uv-build-gentoo-plea"
date: 2026-10-13T20:03:03+0000
summary: "A Gentoo maintainer asks Python projects not to adopt uv-build as their build backend. It is a Rust program of 1.2 to 1.5 MiB per wheel, against 45 KiB for pure-Python flit-core, with almost no benefit for pure-Python packages. Gentoo users must compile it from source, and a dozen packages have lost architecture keywords after a key dependency switched to it."
source: "https://blogs.gentoo.org/mgorny/2026/10/12/a-plea-not-to-use-uv-build/"
source_title: "A plea not to use uv-build"
source_author: "Michał Górny"
source_site: "Michał Górny's blog (Gentoo)"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50076048"
---

**Bottom line:** Gentoo developer Michał Górny asks Python projects not to use `uv-build` as a build backend. He says its drawbacks outweigh its benefits for pure-Python packages. He stresses this is not a complaint about uv or other Astral tools.

**Drawbacks he cites**

- **Size and build cost:** The backend is written in Rust, and its wheels are 1.2 to 1.5 MiB (as of 0.13.0), against about 45 KiB for pure-Python `flit-core`. Gentoo users must build it from source. It had 12 releases in the 30 days before the post, so the rebuilds are frequent.
- **Portability:** Python runs almost anywhere with a C compiler, but Rust support is narrower. Examples he gives are dependencies that skip SPARC, and binaries so large that 32-bit toolchains run out of address space. The Gentoo uv package already carries two hacks for 32-bit builds.
- **Real impact:** A single key dependency switching to uv-build made a dozen more Gentoo packages lose keywords.

**Why he finds the benefits weak**

- Consistency with uv doesn't matter, because PEP 517 lets any frontend use any backend, and flit-core and hatchling both work fine with uv.
- The speed gain is about 100 ms per build, and likely cancelled by downloading the 1.5 MiB wheel.
- Only developers, CI, packagers and source-distribution users run the backend, while regular users install wheels. Source-distribution users are the largest group and the most affected.
