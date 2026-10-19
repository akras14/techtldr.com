---
title: "Let's Encrypt moves to seven validation requests per certificate on Nov 10"
slug: "lets-encrypt-seven-validation-perspectives"
date: 2026-10-19T12:03:23+0000
summary: "Let's Encrypt is adding two remote network perspectives to domain validation, so each validation will make seven requests (one from the primary datacenter, six from remote perspectives, up from four). Issuance succeeds only if the primary and a quorum of remote perspectives see the correct challenge response, which makes BGP-based hijacking of validation harder. The change is driven by CA/Browser Forum Baseline Requirements 3.2.2.9, effective 2026-12-15, which require at least five remote perspectives across at least two Regional Internet Registries. Staging starts October 27 and production rollout is planned for November 10, 2026. Let's Encrypt expects little to no impact on users."
source: "https://community.letsencrypt.org/t/lets-encrypt-is-adding-two-additional-remote-perspectives-for-domain-validation/252166"
source_title: "Let's Encrypt is adding two additional remote perspectives for domain validation"
source_author: "Phil"
source_site: "Let's Encrypt Community"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50150809"
---
