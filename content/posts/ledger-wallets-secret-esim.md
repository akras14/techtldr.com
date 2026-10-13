---
title: "Tampered Ledger Nano X wallets with hidden eSIMs drained about $93M"
slug: "ledger-wallets-secret-esim"
date: 2026-10-13T11:04:27+0000
summary: "Attackers sold modified Ledger Nano X hardware wallets with an extra chip that reads the seed phrase off the screen and an eSIM/LTE modem that sends it to them. Blockchain observers count over 300 drained wallets and roughly $93 million stolen."
source: "https://www.heise.de/en/news/Crypto-thieves-sell-hardware-wallets-with-secret-eSIM-11485188.html"
source_title: "Crypto thieves sell hardware wallets with secret eSIM"
source_author: "Daniel AJ Sokolov"
source_site: "heise online"
hn_url: "https://news.ycombinator.com/item?id=50068508"
---

Attackers intercepted genuine Ledger Nano X devices in the supply chain and sold them with extra hardware inside. Users who generated a seed phrase on one had it stolen, and about $100 million in crypto has been lost, per heise.

- **The Secure Element wasn't touched.** The implant sits between the Secure Element and the built-in screen, picks up the transmitted characters (an embedded "Tempest"-style attack, per Tibane Labs' analysis), and recognizes the displayed seed phrase.
- **Exfiltration over mobile.** An eSIM module, small LTE modem and antenna, powered by the wallet's own battery, send the phrase to the attackers even though the wallet is meant to be offline.
- **Scale:** blockchain observers have identified over 300 affected wallets, about $93M, laundered partly through mixers.
- **Suspect channel:** Malaysia-based reseller Cryptobilis, which reportedly changed owners quietly in August. One tampered unit came via Amazon Japan from a Chinese seller. Ledger says anyone who bought from Cryptobilis in the last three months should stop using the device and move funds, and it has barred the reseller from further sales. It has not confirmed the full circumstances or extent of the damage.
