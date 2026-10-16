---
title: "NIST curve seeds were lrand48 output seeded with 1997–98 timestamps"
slug: "nist-curve-seeds-lrand48-timestamps"
date: 2026-10-16T20:05:00+0000
summary: "Anthropic says Claude worked out, in under half an hour, how the NSA-supplied seeds for the NIST P-192, P-224, P-256, P-384 and P-521 curves were made: not a hash of a joke phrase, but bytes from the C library's lrand48 generator seeded with a Unix timestamp from May 1997 to April 1998. It is not an attack on ECDSA. The simple, reproducible origin makes a deliberate backdoor much less plausible."
source: "https://www.anthropic.com/research/ecdsa-curve-origins"
source_title: "Claude reveals ECDSA curve origins"
source_author: "Anthropic"
source_site: "Anthropic"
source_date: "2026-10-16"
hn_url: "https://news.ycombinator.com/item?id=50122800"
---

**Bottom line:** The long-unexplained seeds behind the NIST elliptic curves turn out to be output from a weak 1990s random number generator, `lrand48`, initialized with the clock. Anthropic frames this as a closing chapter, not a vulnerability: nothing needs to change, and the finding makes a hidden backdoor in the curves less likely.

## What was found

- The folklore was that NSA's Jerry Solinas hashed a humorous phrase. Searching for that phrase went nowhere.
- Claude instead guessed the generator software used a period PRNG and scanned the seeds for its signature. Four seeds had a zero top bit in all 20 words, which is the fingerprint of `lrand48`, since it returns only the top 31 bits of a 48-bit state.
- For the prime-curve seeds, which keep fewer bits per output, Claude recovered the internal state using LLL lattice reduction, a textbook attack on truncated linear congruential generators. All ten prime-curve seeds (five NIST, five ANSI X9.62) matched `lrand48() >> 23`.
- Stepping each state back to its `srand48` initialization gave Unix timestamps between May 1997 and April 1998. The binary-curve seeds fit timestamps that were halved. Each 20-byte chunk of output is tried as a candidate seed. P-256's published seed was candidate 1842.
- Claude also traced some base points. Six curves from May 1997 (P-192 and the ANSI example curves) took theirs from the random number generator in Lenstra's FreeLIP library, seeded with small integers such as 20000 for P-192. The base points for P-224, P-256, P-384 and P-521 remain unexplained.

## Caveats

Not every candidate seed gave a usable curve, and some viable curves were skipped for reasons the team couldn't fully explain (P-256's candidate 876 was also viable but unpublished).

## Also covered by

[Filippo Valsorda](https://words.filippo.io/seeds-solved/), who ran the bounty for this, says the Anthropic team claimed it on October 15 and donated the prize to ISRG, bringing it to $36,864. He reimplemented the algorithm in Go to verify it, and notes there are two undocumented extra acceptance criteria, one of which is still unexplained. He argues the timestamps limit the seed-selection freedom to about 30 bits, which rules out the Bernstein–Lange scenario of targeting a class of weak curves.
