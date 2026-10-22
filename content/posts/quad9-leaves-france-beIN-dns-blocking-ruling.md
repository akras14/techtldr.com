---
title: "Quad9 will pull its DNS resolver out of France on Nov 24 after a beIN piracy ruling"
slug: "quad9-leaves-france-beIN-dns-blocking-ruling"
date: 2026-10-22T16:03:00+0000
summary: "Quad9, the free Swiss nonprofit DNS resolver, says it will stop serving users in France on November 24 rather than comply with a French court order to block resolution of domains beIN says infringe its sports broadcast rights. The court cut the penalty from €580,000 to €1,000 per day, but Quad9 says compliance is technically impossible without either geoIP blocking or blocking the names for every user worldwide."
source: "https://quad9.net/news/blog/au-revoir-france/"
source_title: "Farewell, France"
source_author: "Quad9"
source_site: "quad9.net"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50195959"
---

**Bottom line:** Quad9 will withdraw its service from France on November 24, after a French court ruled against it in a case brought by beIN, a Qatari broadcaster. The order forces Quad9 to block recursive DNS resolution for a list of domains beIN accuses of piracy of sports broadcasts for users in France. The fine was reduced from €580,000 to €1,000 per day, but Quad9 says the issue is principle, not money.

## Why Quad9 says it can't comply

- **No way to identify French users.** The service is free, needs no sign-up, and keeps no user records, so it can't tell which users the ruling covers.
- **Global blocking is unacceptable.** Adding the domains to its shared malware/phishing blocklist would apply French law to every user worldwide, and Quad9 argues it would set a precedent for any country to impose its rules via DNS.
- **GeoIP blocking has failed before.** In a 2021–2023 German case brought by Sony Music, Quad9 geoIP-blocked users, Sony used a VPN to show the blocking could be bypassed, and the court fined Quad9 for contempt. Quad9 eventually won, but it took years and six figures in costs.
- **More cases are pending.** It says it can't survive the accumulated penalties of about 20 other pending cases and future ones like this.

Quad9 says it doesn't condone piracy but argues that recursive resolvers are the wrong place to address it, and that the problem should be handled where the content is hosted.
