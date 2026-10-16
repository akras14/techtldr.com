---
title: "AVIF files are 15–25% smaller than JPEG XL for lossy web photos"
slug: "jpeg-xl-vs-avif-web-benchmarks"
date: 2026-10-16T17:03:20+0000
summary: "Now that JPEG XL works in all major browsers, Jake Archibald benchmarked it against AVIF. For lossy photos AVIF is usually 15–25% smaller at default effort, for screenshots, illustrations and transparency it wins by much more, and he defaults to AVIF 4:4:4 unless an image looks unusually large."
source: "https://jakearchibald.com/2026/jpeg-xl-vs-avif/"
source_title: "JPEG XL vs AVIF for web use-cases"
source_author: "Jake Archibald"
source_site: "JakeArchibald.com"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50120224"
---

JPEG XL is now in all major browsers, and Archibald compared it with AVIF using avifenc 1.4.2, libjxl 0.12.0, and jixel 0.4.2 (a newer JPEG XL encoder), with SSIMULACRA2 as the quality metric. His default for web use is AVIF 4:4:4 at the highest effort he can tolerate, trying JPEG XL via jixel only when the AVIF file looks unusually large.

- **Lossy photos:** the best JPEG XL is typically 15–25% larger than AVIF at default effort, 6–20% at maximum. There are outliers, so try both for a critical image. jixel usually beats libjxl, except with detailed transparency.
- **Screenshots and illustrations:** JPEG XL is 30–90% larger at default effort.
- **Transparency:** libjxl is 31–44% larger at default effort. jixel encodes alpha losslessly, giving files 2–3x AVIF's size.
- **Speed:** AVIF is generally faster to encode per byte saved and faster to decode (JPEG XL takes 2–3x as long at medium quality). libjxl's higher effort levels cost a lot of time and memory for little gain.
- **HDR:** JPEG XL lacks browser-supported gain maps, a significant limitation.
- **Where JPEG XL wins:** lossless (WebP is 9–17% larger on photos, lossless AVIF 30–120% larger) and recompressing existing JPEGs 9–20% smaller with no further loss.
- **Progressive rendering:** works in Chrome and Firefox for JPEG XL. AVIF's is Chrome-only for now, with Firefox in progress.

He notes both formats have improved a lot recently, so the recommendation could change, and advises testing with your own images rather than trusting AI-generated claims.
