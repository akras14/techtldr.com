---
title: "GitLab patches CVSS 10.0 unauthenticated RCE in its regex compiler"
slug: "gitlab-regex-rce-cvss-10"
date: 2026-10-21T20:03:10+0000
summary: "GitLab shipped a critical patch release (19.5.1 and backports to 18.11.13) fixing CVE-2026-110264, an integer overflow in the regular expression compiler that could let an unauthenticated attacker run arbitrary code on the server. It is rated CVSS 10.0 and affects every self-managed CE/EE version from 7.0 up to the fixed releases. GitLab.com and Dedicated are already patched."
source: "https://docs.gitlab.com/releases/patches/patch-release-gitlab-19-5-1-released/"
source_title: "GitLab Critical Patch Release: 19.5.1, 19.4.3, 19.3.6, 19.2.9, 19.1.9, 19.0.10, 18.11.13"
source_site: "GitLab Docs"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50185869"
---

**Bottom line:** self-managed GitLab instances should upgrade now. CVE-2026-110264 is an integer overflow when compiling a specially crafted regular expression, and under certain conditions it allows unauthenticated remote code execution. The vector is network, low complexity, no privileges, no user interaction (CVSS 10.0).

- **Fixed in:** 19.5.1, 19.4.3, 19.3.6, 19.2.9, 19.1.9, 19.0.10 and 18.11.13. The last four are backports.
- **Affected:** all CE/EE versions from 7.0 before those releases.
- **Already safe:** GitLab.com, and Dedicated tenants on 19.3.5-internal0 / 19.4.2-internal0 or later.
- **Detection:** GitLab published two threat detections for self-managed admins, "Onigmo native worker crash" and "Org mode render error burst". The names point at the Onigmo regex engine and Org-mode rendering as the exposed path.
- **Credit:** reported by joaxcar through HackerOne.

The same release fixes a high-severity account takeover (CVE-2026-15723: OmniAuth SAML plus LDAP auto-linking, CVSS 8.2) and an OAuth/OIDC discovery cache poisoning issue (CVE-2026-112212, CVSS 7.5), plus several medium and low issues.
