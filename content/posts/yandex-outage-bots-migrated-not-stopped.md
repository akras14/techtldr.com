---
title: "Yandex.Cloud's bots moved to other hosts after the outage; Russian gambling-site bot traffic rose 1.4%"
slug: "yandex-outage-bots-migrated-not-stopped"
date: 2026-10-14T21:02:42+0000
summary: "Cloudflare data on the early-October Yandex outage (reported drone strikes on data-center regions) shows malicious automation relocated rather than stopped. Bot traffic from Yandex.Cloud fell about 40%, but the same bots, with identical JA4 fingerprints, paths, headers and User-Agents, reappeared on Radist and Selectel. Russian bot traffic against one gambling customer went from 167.7M to 170.1M requests (+1.4%). Cloudflare found no evidence that social media manipulation was disrupted, though it says its data alone can't rule out a link."
source: "https://blog.cloudflare.com/yandex-outage/"
source_title: "Yandex outage: data shows that instead of going dark, malicious bots went on the move"
source_site: "Cloudflare Blog"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50094673"
---

**Bottom line.** Cloudflare's traffic data says the Yandex outage did not dent malicious automation: operators shifted workloads to other Russian hosting providers within a day, and overall Russian bot volume against the sampled target stayed flat.

**What dropped.**
- Traffic from the Yandex Search ASN (AS13238) fell mostly because of search crawling: 78.1% of sampled requests were verified YandexBot. News and publishing sites saw a 44% drop from these networks, so the visible effect was less content indexing by Yandex Search.
- Bot traffic from the Yandex.Cloud ASN (AS200350) fell about 40%. WordPress plugin probing fell 90% and scanner-identified requests fell 93%. Cloudflare customer blocks on that ASN fell 70%.
- Russia-origin bot requests fell from 50.4% to roughly 42% of Russia-origin HTTP requests, roughly in proportion to the Yandex traffic that went offline. Global bot share stayed near 39.4%.

**What moved instead.** Nearly all Yandex.Cloud bot requests targeted one gambling customer account. After the October 8 disruption they dropped to zero there, and identical requests (same JA4 TLS fingerprint, uncommon paths, headers and custom User-Agents) appeared from other Russian ASNs including Radist and Selectel. Total Russian bot requests to gambling accounts went from 167.7M to 170.1M.

**Social media.** Traffic from Russian IPs to the social platform endpoints Cloudflare checked ("post", "reply", "like") changed by under 6%, and global traffic by about 1%. Cloudflare says the data doesn't support claims that the strikes crippled social media manipulation, with the caveat that it can't conclusively rule out a connection without data from outside its network.
