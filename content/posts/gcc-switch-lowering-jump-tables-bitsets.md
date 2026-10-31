---
title: "GCC jump-tables a switch only at ~8 table slots per comparison (2024)"
slug: "gcc-switch-lowering-jump-tables-bitsets"
date: 2026-10-31T10:02:47+0000
summary: "Xoranth's walkthrough shows GCC lowers a switch three ways: a jump table, a bitset test, or a binary comparison tree, picked by cost heuristics. At -O3 a jump table needs the value range to be at most about 8x the number of comparisons (2x under -Os) and at least 5 labels on x64/Arm64; sparse cases fall back to a tree, and a dense run with an outlier gets a table plus one extra branch. Small non-contiguous sets of under 64 bits can become a constant bitmask tested with bt, which is GCC-specific. Some results look odd, such as a 20-label switch compiled to a 10-comparison tree."
source: "https://xoranth.net/gcc-switch/"
source_title: "Switch lowering in GCC"
source_author: "Xoranth"
source_site: "xoranth.net"
source_date: "2024-01-14"
hn_url: "https://news.ycombinator.com/item?id=50314085"
---

## Bottom line

Xoranth's walkthrough shows GCC lowers a `switch` three ways: a jump table, a bitset test, or a binary comparison tree. It picks among them with cost heuristics. Most of this also holds for Clang, but the bitset lowering is GCC-specific.

## How GCC chooses

- **Jump table:** used when the case values are dense. Adjacent labels merge into ranges, so GCC counts comparisons, not labels. At speed optimization the value range may be at most about 8x the number of comparisons (2x for `-Os`), with at least 5 labels (`case_values_threshold` on x64 and Arm64). Offset cases (97–101) just subtract the offset first.
- **Dense subset plus outlier:** cases 1–5 plus 100 get a jump table for 1–5 and a separate branch for 100.
- **Bitset:** for small non-contiguous sets, GCC packs the case values into a constant mask and tests it with `bt`. The range must fit in a register (under 64 bits) with at most 3 distinct targets, and it needs 3, 5 or 6 labels for 1, 2 or 3 targets.
- **Decision tree:** everything else becomes a binary tree split by probability, which comes from profile data or `__builtin_expect_with_probability`.

## Examples

- 0, 7, 14, 21, 34 gets a jump table (34 is within 7x the 5 labels).
- 0, 8, 16, 24, 40 gets a comparison tree (sparser than the threshold).
- Nine values from `'0'` to `'E'` become a bitset (constant 2753205 with `bt`).
- A 20-label block spanning 0–87 becomes 10 comparisons, since 10 x 8 < 88.

## Implementation

The work happens in `pass_lower_switch` (`analyze_switch_statement`, in `tree-switch-conversion.cc`). Labels become clusters with a range, target edge and probability. Bitset clusters are chosen first, then jump tables, both by dynamic programming, and the leftovers go into the tree.

The author notes some choices look non-intuitive, and that GCC misses simplifications such as dividing by 8 before the jump in the sparse case.
