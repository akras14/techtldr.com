---
title: "Northflank breach: assume all environment variables are decrypted"
slug: "northflank-env-vars-decrypted-breach"
date: 2026-10-30T16:03:02+0000
summary: "Northflank is in a major outage from a security incident, and its status page tells all users to assume their environment variables were decrypted and to rotate every key immediately. Affected secrets include those on workloads, secret groups and global secrets, and multiple users report their AI keys have already been used."
source: "https://status.northflank.com"
source_title: "Northflank - Status"
source_site: "status.northflank.com"
source_date: "2026-10-30"
hn_url: "https://news.ycombinator.com/item?id=50305025"
---

**Bottom line:** if you run anything on Northflank, rotate every secret now. The company's status page says to assume all environment variables have been decrypted.

- **Scope:** secrets on workloads, secret groups and global secrets. Networking, certificates, services, addons, jobs, builds and logs/metrics show a major outage, while the app, API, marketing site and docs show operational.
- **Evidence of abuse:** the 3:12 PM UTC update says multiple users confirm their AI keys have been used.
- **Timeline posted:** a Wiz threat alert at 11:43 UTC, engineers reviewing it a minute later, a first network policy patch at 12:02, more patches at 12:50 and 13:05, and Wiz incident-response support requested at 13:00.
- **Escalation:** the wording moved from "rotate environment variables" (1:40 PM) to "assume all decrypted, disable/rotate all keys" (1:58 PM) to "begin rotation immediately" (3:12 PM).

The page gives no root cause yet. It is a live status page and will change.
