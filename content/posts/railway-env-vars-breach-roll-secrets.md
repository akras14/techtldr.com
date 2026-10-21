---
title: "Railway says attackers may have decrypted customer environment variables"
slug: "railway-env-vars-breach-roll-secrets"
date: 2026-10-21T23:02:44+0000
summary: "Railway detected anomalous activity on systems that store customer environment variables and says an unauthorized party may have accessed them and, for some, obtained the decrypted contents. Roll every environment variable and registry credential stored in Railway now, and check logs for unusual activity. All deployments are paused while it investigates."
source: "https://blog.railway.com/p/security-disclosure-wednesday-october-21st"
source_title: "Security Disclosure: Wednesday October 21st"
source_author: "Jacob Cooper"
source_site: "Railway Blog"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50188228"
---

If you deploy on Railway, rotate your secrets now. Railway says that earlier on October 21 it detected anomalous activity involving systems that store customer environment variables. Its investigation so far indicates an unauthorized third party may have accessed encrypted environment variables and, for some of them, obtained the decrypted contents. Those can include API keys and database or registry credentials.

- **Scope:** Railway says the impact may be limited to a subset of users, but recommends everyone act as a precaution while it finishes its review.
- **What to do:** roll all environment variables, including API keys, and any private registry credentials stored in Railway. Then review logs for those services for unusual activity.
- **Deployments paused:** all deployments are paused during the investigation, so you can't roll out new versions until builds are restored.
- **Wider campaign:** Railway believes this is part of a series of recent attacks on platform companies aimed at compromising AI inference providers. It has told those upstream providers to revoke keys proactively.
- **Next steps:** Railway will email customers it confirms were affected and post updates on the announcement and on status.railway.com.
