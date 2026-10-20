---
title: "1,331 Danfoss fridge dashboards were open in France before the attack"
slug: "france-fridge-controllers-exposed-online"
date: 2026-10-20T01:02:29+0000
summary: "Before attackers pushed refrigeration systems in 400+ French supermarkets into defrost mode on October 11-12, 2026, IPinfo's scans found 1,331 Danfoss StoreView dashboards exposed to the internet in France, more than the other 72 countries combined (1,197). 85% ran software with known flaws, and most had been exposed for at least six months."
source: "https://ipinfo.io/blog/france-fridges-hack"
source_title: "France's Supermarket Fridges Were Vulnerable Online for Months"
source_author: "João Tomé"
source_site: "IPinfo"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50159616"
---

Hundreds of French supermarkets had refrigeration controllers pushed into defrost mode overnight on October 11-12, 2026. Some units reached 50°C, some equipment caught fire, and stores threw away food. IPinfo's weekly internet scans show the exposure was there for months: the October 6-7 sweep found 2,528 Danfoss StoreView dashboards online worldwide, 1,331 (53%) of them in France. Poland was next with 179.

- **Patching wasn't the difference.** 85% of French dashboards (1,136) ran software with known flaws, versus 84% elsewhere. 527 were older controllers Danfoss no longer supports, and only 195 had every known fix.
- **Long-lived exposure.** 1,019 of the French dashboards were at the same address on March 30. The share of newer controllers with all fixes only rose from 14% to 24% in that time.
- **Why France is unclear.** Almost all (98%) sit on telecom networks rather than clouds, which fits controllers on a store's own connection with remote access opened for technicians or head office.
- **The attack.** Danfoss blamed "legacy" AK-SM 800 and AK-SC 355 controllers. CERT-FR's alert later added Copeland and Dixell XWEB supervisors and says campaigns are probably ongoing. Nobody has said who is responsible or how they got in, and the Paris prosecutor has opened an investigation.
- **Afterwards.** Only 316 of the 1,331 French dashboards were still online on October 14.
