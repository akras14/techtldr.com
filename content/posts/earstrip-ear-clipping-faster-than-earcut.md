---
title: "Earstrip triangulates 3.7M map polygons in 489 ms, 6x faster than Delaunay"
slug: "earstrip-ear-clipping-faster-than-earcut"
date: 2026-10-22T11:04:13+0000
summary: "Globus replaced the constrained Delaunay triangulator in its map engine with an ear clipper and published it as earstrip, a single C++ header plus a WebAssembly/npm build with earcut's calling convention. On 3.74M Warsaw-region polygons it takes 489 ms versus 2,931 ms for Delaunay, is 14% faster than earcut.hpp natively, and runs 1.4 to 2.1 times as fast as earcut.js in the browser."
source: "https://globus.software/blog/2026/10/16/cutting-map-polygons/"
source_title: "Faster than earcut: triangulating 3.7M map polygons in 489 ms"
source_author: "Evgen Bodunov"
source_site: "Globus Software"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50193170"
---
Globus open-sourced the polygon triangulator from its map engine as **earstrip** (ISC license, same as earcut): one C++ header and an npm WebAssembly build that takes the same arguments as earcut. A GPU only fills triangles, and maps are mostly polygons, so every outline has to be cut first. Swapping their constrained Delaunay code for ear clipping took the 3,740,425 polygons of one Masovian map zoom level from 2.93 s to 489 ms on one M4 Pro core.

## How the speedup builds up

The author rebuilt the clipper one idea per commit and timed each on the same polygons:

- **Naive ear clipping:** 1,119 ms. Already 2.6x faster than Delaunay, but quadratic on big polygons (the 1,123 largest were slower than Delaunay).
- **Test only reflex vertices ("dents"):** 882 ms. Any vertex inside an ear's triangle implies a reflex one is too, and the dent list only ever shrinks.
- **Convex shortcut:** 880 ms. Convex polygons need no ear tests; a left-turn check alone is fooled by stars, so it also counts x-direction changes.
- **Write triangle strips:** 762 ms. Zig-zagging cuts indices from 64.5M to 35.8M.
- **Bucket the dents** on large polygons: 697 ms.
- **Branch-free counting pass** over dents inside the triangle: 519 ms.
- **Re-test only the marked dents:** 489 ms.

## Holes and failure cases

Holes are stitched to the outer ring with bridges (Eberly's rightmost-vertex ray method), with special handling for integer-grid rays through vertices and courtyards touching the outer wall. Rounding to the tile grid can leave a polygon with no ears at all, so earstrip falls back to constrained Delaunay when ears run out, when the cut area is wrong, or when a polygon has 128+ rings (one world-map polygon with 11,675 holes takes 606 ms by ears, 17 ms by Delaunay). Their map generator now snap-rounds polygons so ear clipping can cut all of them.

## Versus earcut

- Native C++: 489 ms vs 557 ms for earcut.hpp 3.2.4 (14% faster).
- WebAssembly (V8): 601 ms vs 930 ms; npm package with one call per polygon, 766 vs 1,103 ms.
- World-map tiles (56,472 polygons, many holes): 459 ms vs 2,892 ms.
- On polygons the ears can't cut exactly, earcut's output differs from an even-odd fill on 31% of pixels on average, earstrip's on 0.15%.

The benchmarks are the author's own, on their map data.
