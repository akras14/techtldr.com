---
title: "ESP32-S3 SIMD doubles FAST corner detection to 11.2 MP/s on a $2 chip"
slug: "esp32-s3-simd-fast-corner-detector"
date: 2026-10-26T17:02:45+0000
summary: "Using the ESP32-S3's little-known 128-bit SIMD instructions, the author wrote an assembly FAST feature detector that raises throughput from 5.1 to 11.2 MP/s (about 6 ms per QVGA frame), enough for a 30 fps VGA stream. Along the way the ISA's quirks forced workarounds: no misaligned loads, signed-only comparisons, and saturating arithmetic."
source: "https://shraiwi.com/blog/simd-fast-esp32s3/"
source_title: "SIMD-accelerated computer vision on a $2 microcontroller"
source_author: "shraiwi"
source_site: "shraiwi"
hn_url: "https://news.ycombinator.com/item?id=50236681"
---

The ESP32-S3 has a small set of SIMD instructions that few people use. The author wrote a FAST corner detector in assembly around them. It runs about 2.2x faster than the reference implementation: 11.2 MP/s versus 5.1 MP/s, or roughly 6 ms per 320x240 frame. That is enough to process a 30 fps VGA stream on a roughly $2 chip. Along the way the author also wrote a small register allocator, `basm`.

## How it works

The pre-test samples one pixel in each cardinal direction and checks whether at least three differ from the center by more than a threshold. Each 128-bit register holds sixteen 8-bit pixels, so the code evaluates sixteen candidate pixels at once.

## ISA quirks and the workarounds

- **No misaligned loads.** The east and west neighbor blocks are misaligned relative to the aligned center block. The fix is an instruction that extracts a slice from two concatenated registers, applied to the adjacent aligned blocks.
- **Signed-only comparisons.** Pixels are unsigned, and reinterpreting them as signed breaks the ordering above 127. They need shifting from [0, 255] to [-128, 127].
- **Saturating arithmetic.** Subtracting 128 would clamp instead of wrapping. Because -128 is the top bit's place value, XOR with `0x80` does the same job. Each pixel is XORed with `0x80` on load.

The author also wrote a SIMD scoring function but doesn't cover it in the post.
