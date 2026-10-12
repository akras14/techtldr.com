---
title: "HashiCorp archives Boundary Community Edition, with no security patches planned"
slug: "hashicorp-archives-boundary-ce"
date: 2026-10-12T02:02:23+0000
summary: "HashiCorp archived the open source Boundary repository on October 8, 2026. Boundary CE is no longer under active development, and development will continue only in the commercial HCP Boundary and Boundary Enterprise products."
source: "https://github.com/hashicorp/boundary"
source_title: "hashicorp/boundary README"
source_author: "HashiCorp"
source_site: "GitHub"
source_date: "2026-10-08"
hn_url: "https://news.ycombinator.com/item?id=50049498"
---

HashiCorp archived the open source Boundary repository on October 8, 2026. Boundary Community Edition (CE) is no longer under active development, and future work will focus on the commercial HCP Boundary and Boundary Enterprise offerings.

The README spells out what that means:

- The repository is read-only. It accepts no new issues, pull requests or community contributions.
- Existing source code and previously released binaries stay available, and self-managed installs can keep running.
- No new CE releases, maintenance updates, bug fixes or security patches are currently planned.
- Teams still running CE are told to check whether it meets their operational, security and compliance requirements.

HashiCorp points users to HCP Boundary or Boundary Enterprise for production-scale governance, compliance, session recording and support. It says the paid products will also cover "human and agentic access" going forward.

For anyone running Boundary CE as an access proxy, the practical risk is the missing security patches. The choices are to migrate to the paid product, fork the archived code, or move to another access tool.
