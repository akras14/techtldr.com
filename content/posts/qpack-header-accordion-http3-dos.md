---
title: "QPACK header accordion: 7 of 10 HTTP/3 stacks can be OOMed by one client"
slug: "qpack-header-accordion-http3-dos"
date: 2026-10-22T04:04:00+0000
summary: "FoxIO found that seven of ten tested HTTP/3 implementations let a remote, unauthenticated client force the server to allocate 15x to roughly 4000x more memory than it receives, by abusing QPACK header decompression. One connection can OOM a default-configured server. quic-go, Cloudflare quiche, lsquic and Wireshark are patched; Bun, hyperium h3 and aioquic/mitmproxy were not as of Oct 20, 2026."
source: "https://foxio.io/blog/header-accordions-qpack-amplification-attacks-against-http-3-servers"
source_title: "Header Accordions: QPACK amplification attacks against HTTP/3 servers"
source_author: "Sébastien Féry"
source_site: "FoxIO"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50188612"
---

**Bottom line:** Header limits in many HTTP/3 servers are missing, incomplete or applied after decoding, so a small QPACK-compressed HEADERS frame can expand into hundreds of thousands of in-memory header objects. Seven of ten implementations tested are vulnerable, and one unauthenticated connection can exhaust memory on a default configuration.

**Two attack variants.** In the first, a ~1 MB HEADERS frame holds about 160,000 uniquely named tiny headers. In the second, the attacker inserts one ~4 KiB entry into the dynamic table and references it thousands of times with 1-byte indexed references, each of which forces a full copy (a 4064:1 ratio).

**Why they work.** RFC 9114 says field-list size is the uncompressed name plus value plus 32 bytes per field. Implementations got this wrong by checking the compressed size, decoding everything before checking, omitting the 32-byte overhead, defaulting to unlimited, or never enforcing the configured setting.

**Status as of Oct 20, 2026:**

- Fixed: quic-go 0.57.0 (CVE-2025-64702, ~15x), Cloudflare quiche 0.29.3 (CVE-2026-12523, ~20x), lsquic 4.9.0 (~600-1800x), Wireshark 4.6.2 / 4.4.12 (CVE-2025-13945, crash).
- Not fixed: Bun 1.4.3 (~4000x), hyperium h3 (thread panic), aioquic / mitmproxy (~20-30x).
- Not affected: Google quiche, IIS, Envoy, ngtcp2/nghttp3.

**Mitigations for unpatched stacks:** don't expose Bun HTTP/3 servers, run mitmproxy with `--set http3=false`, and set a reasonable `max_field_section_size` with the h3 crate. The fix pattern is to enforce a cumulative budget incrementally while decoding, as Google quiche and IIS already do.
