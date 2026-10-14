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

**Also covered by:** [Kim Zetter, "Trezor Wallets Also Compromised with Implants"](https://www.zetter-zeroday.com/trezor-wallets-also-compromised-with-implants/) (2026-10-13). Hardware hacker Joe Grand found implants in a Trezor Model T and a Trezor One bought from unauthorized sellers (Facebook Marketplace, a TikTok shop) and reported them to Trezor in August. Attackers swapped the microcontroller for one running modified open-source firmware that generates seed phrases from a small preset list, so they can guess the recovery seeds. Trezor says there is no evidence its devices were compromised, though it is cutting its list of authorized resellers.

**Also covered by:** [Kim Zetter, "Who Is Planting Backdoors in Ledger Crypto Wallets?"](https://www.zetter-zeroday.com/who-is-planting-backdoors-in-ledger-crypto-wallets/) (2026-10-13). Adds that implants have also been reported in Nano X units bought new at MediaMarkt stores in Germany and in Ledger Flex and Stax devices, so authorized resellers are affected too. The implants evolved from hand-soldered wires (found by Joe Grand in March) to a colour-matched flex circuit with a custom antenna marked `AC-A2016-NANO-V1.1`, which researcher Mark Karpelès reads as a production run. The $102M figure covers about 315 accounts tied to Cryptobilis devices. Ledger says its own systems show no compromise and blames resellers restocking returns. ([HN discussion](https://news.ycombinator.com/item?id=50084838))
