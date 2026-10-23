---
title: "Claude proved 239 fuzz entry points crash-free across 123 libraries"
slug: "claude-proves-123-libraries-no-crash-riscv"
date: 2026-10-23T01:02:05+0000
summary: "Anthropic's Frontier Red Team, with Theorem, had Claude write machine-checked Rocq proofs that 239 fuzz-test entry points in 123 OSS-Fuzz libraries never crash on any input under 64 MB. The proofs work on compiled RISC-V binaries (about 180 million lines of Rocq in total), and attempting them surfaced hundreds of bugs, some that fuzzers would be unlikely to find. The guarantees hold only under stated assumptions: single-threaded static binaries on RISC-V with Linux, and trust in the formal models and the proof checker."
source: "https://www.anthropic.com/research/formal-verification"
source_title: "LLMs are making formal software verification feasible at a much larger scale"
source_author: "Anthropic Frontier Red Team"
source_site: "anthropic.com"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50202217"
---

**Bottom line:** Anthropic argues that LLMs have made formal verification practical for real code. In a collaboration with Theorem, Claude wrote proofs that 239 fuzz-test entry points across 123 open-source libraries (from Google's OSS-Fuzz) always exit normally, with no memory-safety errors or failed assertions, for every input under 64 MB. Verification has historically cost far more than writing the code; the post cites seL4 at 2 person-years to build and 20 to verify.

## How it works

- Each library's existing fuzz test becomes the spec: "this test never crashes", stated over the compiled RISC-V executable using Theorem's formal models of RISC-V and Linux processes.
- Claude writes the proof and the Rocq proof checker validates it, so almost none of the roughly 180 million lines of proof needs human review. A typical proof runs about 600,000 lines, on binaries with a median of 18,000 instructions.
- What you still have to trust: the RISC-V and Linux models, the exact theorem statements, and Rocq itself.
- When the proof attempt hit a bug, Claude patched the code, regenerated the spec and continued.

## What it found

Hundreds of bugs, many unlikely for a fuzzer to reach because fuzzers favor small inputs:

- A counter underflow in a decompressor leading to an out-of-bounds write, triggered only by a roughly 20 MB input with a rare layout.
- An out-of-bounds read in an image decoder needing a 64 MB input and over 2 GB of memory.
- A read bug needing a specific call sequence across two handles to one file.
- A rediscovery of a libexpat CVE that needs deeply nested input.
- A logic bug that never crashes: a feature meant to restrict which files an app can load was not always enforced.

## Other experiments

- A Claude-written spec of RFC 1950, with the Linux kernel's zlib proven against it at the x86 binary level; only minor unexploitable corner-case bugs turned up.
- A Python interpreter in Rust, verified with Verus, matched CPython on 99.95% of 1.7 million random programs.
- Verification attempts on a widely used PDF reader found as many security bugs in a week as are typically reported in a year, plus a decade-old latent logic bug elsewhere.

## Caveats

- The proofs assume single-threaded static binaries with a fixed memory budget on RISC-V and Linux.
- It does not mean the libraries are bug-free, only that remaining bugs lie in the trusted components or outside the proven scope.
- Proofs can take hundreds of millions of tokens, so cost-efficiency is the main open problem.
- Anthropic is publishing the trusted components and a subset of proofs, and Theorem offers to run the pipeline on other open-source projects and publishes a Claude skill for verifying your own code.
