---
title: "Compile-time perfect hashing makes C++ string lookups 13x faster than unordered_map"
slug: "perfect-hash-cpp-lookup-tables"
date: 2026-10-28T12:13:07+0000
summary: "For a fixed set of string keys, a compile-time perfect hash plus a single 16-byte vector comparison looks up a key in about 1.3 ns (roughly 5 cycles) on an Apple M3 Max, against 7.7 ns for gperf, 10.9 ns for Abseil's flat_hash_map and 16.6 ns for std::unordered_map. The C++23 library is a single header."
source: "https://lemire.me/blog/2026/10/26/billion-query-per-second-lookup-tables-in-c/"
source_title: "Billion-query-per-second lookup tables in C++"
source_author: "Daniel Lemire"
source_site: "lemire.me"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50271088"
---

**Bottom line:** If your string keys are known at compile time (HTTP methods, header names, keywords), you shouldn't pay for collision handling at runtime. Lemire and Francisco Geiman Thiesen built a C++23 single-header library that finds a perfect hash at compile time and checks the match with one vector compare, reaching about 5 cycles per lookup.

- **Hash.** gperf-style: slot = length + a few `asso[char]` values from selected key positions, masked to a power-of-two table. The association values are found by a bounded greedy search so it fits within constexpr limits. Hashing can take as few as four instructions.
- **Comparison.** Load 16 bytes unconditionally, zero the bytes past the key's end with a shuffle (`tbl` on ARM NEON), and compare against the zero-padded stored key in one vector op. That replaces roughly 40 instructions and 8 branches with 5 instructions and no branch. To stay safe, it takes a slower path if the load would cross a page boundary.
- **Numbers (Apple M3 Max, AppleClang 15).** 20 standard HTTP header names: 1.3 ns per lookup vs gperf 7.7, Abseil flat_hash_map 10.9, std::unordered_map 16.6. Across several key sets (6 URL protocols to the S&P 100 tickers): 1.2 to 2 ns, 3 to 6x faster than gperf and 7 to 13x faster than std::unordered_map, and 17x faster than gperf on JavaScript reserved words. On an M4 with URL protocols it exceeds a billion lookups per second.
- **Usage.** `make_perfect_map<kv<"GET", Method::GET>, ...>()` yields a constexpr map; no external generator needed. Library: github.com/ConstexprCore/perfect_hash. Presented at P99 CONF 2026.
