---
title: "OpenAI's 'faster FFT' only wins at n beyond 10^100, argues Ben Recht"
slug: "openai-faster-fft-galactic"
date: 2026-10-13T04:03:10+0000
summary: "Ben Recht looked at the faster Fourier transform claimed in OpenAI's recent math-paper dump and says it isn't a practical result. The algorithm is asymptotically faster only in big-O terms, with hidden constants so large (he cites about 10^100 at N=10^12) that the classic FFT stays far faster at any realistic size, and it assumes exact arbitrary-precision complex arithmetic in a single step. He sees this as a failure of theory's models, not a breakthrough."
source: "https://www.argmin.net/p/faster-fourier-transforms"
source_title: "Faster Fourier Transforms"
source_author: "Ben Recht"
source_site: "arg min"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50065932"
---

**Bottom line:** Ben Recht, a professor who blogs at arg min, argues the "faster FFT" in OpenAI's batch of AI-generated math papers is a galactic algorithm: technically asymptotically better, practically irrelevant.

**The argument**
- The Cooley-Tukey FFT needs under 5 n log2 n operations. OpenAI's claim shaves the log term slightly in big-O terms.
- The hidden constants are enormous: Recht cites estimates of about 10^100 for N=10^12. Even at n=10^100, the classic FFT would be around 10^99 times faster.
- The model of computation assumes exact complex arithmetic, with numbers whose magnitudes grow superpolynomially and multiplications that cost one step. Stored as bits, that implies exponential memory. No real computer works this way.
- This fits prior lower-bound work: Morgenstern (1973) showed ½ n log n is a floor with bounded coefficients, and Ailon showed going below n log n causes severe numerical instability. An unrealistic model is what lets the new result escape these bounds.
- Recht says other results in the dump, such as an O(n^2.25) matrix multiplication, have the same issue; its hidden constant can't even be estimated. He adds that such papers might still be accepted at STOC or FOCS if better written.

**Caveats.** Recht admits he didn't read the paper closely, and that he used ChatGPT to extract its key facts. He also says he doesn't know whether the theorem is true. The piece is an opinionated blog post, not a formal rebuttal.
