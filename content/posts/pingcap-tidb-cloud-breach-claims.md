---
title: "FulcrumSec claims 10.6 TB taken from PingCAP's TiDB Cloud, undetected for two months"
slug: "pingcap-tidb-cloud-breach-claims"
date: 2026-10-21T03:02:12+0000
summary: "A group calling itself FulcrumSec says it took 10.6 TB from PingCAP, the company behind TiDB Cloud, and has published a 128 GB first release. It claims the haul includes the production control plane, 115,308 plaintext customer API keys, 20.6 million customer SQL statements, four years of gateway connection logs and 413 private repositories. The claims come from the attackers and are unverified; PingCAP's response isn't in the post."
source: "https://fulcrumsec.vg/pingcap/"
source_title: "The PingCAP TiDB Cloud Leaks"
source_author: "FulcrumSec"
source_site: "fulcrumsec.vg"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50173716"
---

A group calling itself FulcrumSec says it exfiltrated 10.6 TB from PingCAP, which operates the multi-tenant TiDB Cloud database service, and has published a first 128 GB release as torrents. All of this is the attackers' own account; the post carries no response from PingCAP and none of it is independently verified here.

**What they claim to hold**
- The full production control plane database: 180,046 tenant organisations, about 403,000 unique emails, and 115,308 console API keys stored in plaintext (42,612 reportedly still active).
- 20.6 million raw SQL statements captured from the SQL Editor and Chat2Query features, 102 million gateway connections over four years, and 5 million customer server IPs.
- 413 private source repositories, plus Jira (377k issues), CRM, Stripe, NetSuite and AWS cost data tagged per tenant.
- Customer credentials pasted into support tickets, which they say included live cloud keys, database connection strings and GitHub tokens belonging to customers.

**How they say it happened**
They say they exfiltrated for nearly two months, and that credentials committed in PingCAP's own repositories (GCP service-account keys, an Azure service principal) let them set up a replication feed of the control-plane database. They report 88 consecutive six-hourly exports between 8 and 30 September with no detection event.

**Code and process findings they cite**
- A 2022 internal ticket (SECURITY-22) describing that any role in a customer org, including billing-only, could reset the root password of every cluster in the org.
- A billing service that trusts an unverified `X-ACCOUNT-USER-ID` header.
- A shared crypto library using AES-CBC with a static IV derived from the key prefix, so stored downstream-database passwords depend on a single server-wide secret.

Anyone running TiDB Cloud should treat their API keys, cluster credentials and anything pasted into a support ticket as potentially exposed and rotate them.
