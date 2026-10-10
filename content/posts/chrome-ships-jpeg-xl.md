---
title: "Chrome 155 ships JPEG XL, decoded by a Rust library instead of C++"
slug: "chrome-ships-jpeg-xl"
date: 2026-10-07T23:10:00-07:00
summary: "Chrome 155 adds native JPEG XL decoding. Google rebuilt the decoder in Rust (jxl-rs) so a new image format doesn't mean a new memory-safety attack surface, and says it got there without giving up speed. The deciding factor was years of steady developer demand through the Interop process."
source: "https://developer.chrome.com/blog/jpeg-xl-in-chrome"
source_title: "Shipping JPEG XL in Chrome"
source_author: "Luca Versari, Moritz Firsching, Philip Jägenstedt"
source_site: "Chrome for Developers"
source_date: "2026-10-06"
---

Chrome 155 adds native JPEG XL decoding. Google rebuilt the decoder in Rust (jxl-rs) so a new image format doesn't mean a new memory-safety attack surface, and says it got there without giving up speed. The deciding factor was years of steady developer demand through the Interop process.

## What JPEG XL gets you

- **Smaller files.** Google cites roughly 30–50% better compression than classic JPEG.
- **Lossless mode**, plus built-in HDR support.
- **Lossless JPEG transcoding.** Existing JPEGs can be repacked as `.jxl` and converted back bit-for-bit.
- **Fine-grained progressive decoding**, so a usable image appears before the whole file arrives.

Google's advice is to test both AVIF and JPEG XL on your own images. They expect JPEG XL to win mainly on high-fidelity or lossless photographic images, and where progressive loading matters.

## Why Rust

Image decoders are a favorite target for browser exploits: they parse complicated binary data straight off the network, inside the renderer. Historically, C++ decoders have produced a steady stream of out-of-bounds reads, heap overflows, and use-after-free bugs.

Chrome already relies on sandboxing, but the team treats that as a second line of defense. Instead of shipping the C++ reference decoder (libjxl), they integrated **jxl-rs, a pure-Rust implementation**, to remove that whole class of bug at the source.

## Safe and fast

The authors' argument: a memory-safe decoder is an easy choice only if it's roughly as fast as the unsafe alternative. To get there:

- **SIMD without `unsafe`.** A Rust language feature (`target_feature_11`) had to be stabilized first, which lets code use vector instructions without unsafe blocks.
- **A SIMD abstraction layer** (`jxl_simd`), modeled on Google's C++ Highway library, keeps the remaining unsafe operations in a few heavily reviewed spots.
- **libjxl's optimizations were carried over**, including a processing pipeline that minimizes data copies. Performance is tracked across hardware on a public dashboard.

They checked the code with fuzzing and AI-assisted review, and report **no memory-safety bugs found at any point** in jxl-rs's history. They present this as more evidence of what Rust buys you.

## Why now

The post credits the decision to consistent developer feedback. JPEG XL was a popular Interop proposal in 2026 and in several earlier years. Chrome also took part in the Interop 2026 JPEG XL investigation to make sure cross-browser tests cover the format's features and pass in Chrome.

## Bottom line for developers

If you serve high-quality photography, JPEG XL is now worth benchmarking against AVIF in Chrome. Lossless transcoding also makes it a low-risk way to shrink an existing JPEG library.
