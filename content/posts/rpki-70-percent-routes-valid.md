---
title: "RPKI-valid routes rose from 54% to 70% in a year, 60% of the gain from China"
slug: "rpki-70-percent-routes-valid"
date: 2026-10-16T02:03:00+0000
summary: "IPinfo measured the share of routed IP prefixes covered by valid RPKI records at 70.3% in September 2026, up from 54.0% a year earlier. China alone went from 3.7% to 83.0% of its IPv4 addresses in valid routes. RPKI works where it's checked: invalid routes are seen by about 2 vantage points, against roughly 596 for valid ones. But it only checks who may originate a prefix, and 16.2% of covered IPv4 routes have a ROA looser than the route announced, leaving room for forged-origin hijacks like the August 2026 Hetzner incident."
source: "https://ipinfo.io/blog/rpki-protects-70-internet-routes"
source_title: "RPKI Now Protects 70% of Internet Routes, Up From 54% a Year Ago"
source_author: "João Tomé"
source_site: "IPinfo"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50112330"
---

**Bottom line:** IPinfo measured the share of routed prefixes with valid RPKI records at 70.3% in September 2026, up from 54.0% a year earlier (16.4 points, against 3.9 and 3.2 in the two prior years). RPKI is doing its job where networks check it, but it only covers route origin, and loose ROAs leave a real gap.

**What drove the jump**
- China went from 3.7% to 83.0% of its IPv4 addresses in valid routes, almost 60% of the global gain by address count. Outside China the rise was about 5 points, in line with prior years. Egypt (23% to 99%), Kazakhstan (16% to 84%) and Mexico (37% to 78%) also moved a lot.
- IPv6 overtook IPv4: 79% of IPv6 prefixes valid versus 68% of IPv4.
- Most unprotected IPv4 space is in the US (nearly 60%), and about a quarter of it belongs to the Department of Defense.

**It works**
- A valid prefix is seen by about 596 collector peers, one with no record by 595, and an invalid one by just two. Networks are dropping invalid routes.
- Raw "invalid" counts mislead: Spain's 28.2% invalid prefixes shrinks to 0.057% once weighted by how many vantage points see them.

**Where it falls short**
- RPKI validates who may originate a prefix, not the path. On August 28, 2026, a forged-origin /24 inside Hetzner's /16 ROA (maxLength allowed /24) was RPKI-valid and won on longest-prefix match, enough to get a real TLS certificate and push a malicious Virtualizor update, per Kentik.
- 16.2% of covered IPv4 routes (20.9% of IPv6) have a ROA looser than the announced route with no more-specific route filling the gap. RFC 9319 recommends leaving maxLength unset.
- Coverage can silently lapse: SEACOM lost four of six ROAs on January 1, dropping Mauritius from 94% to 69%.
- Counting individual records as "failed" can overstate lost protection, since stale certificate links don't affect validated route lists.
- ASPA, which signs authorized upstream providers to validate paths, is starting to arrive.
