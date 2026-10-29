---
title: "Alpine rebuilt its GitLab after an attacker nearly landed a CVSS 10 exploit"
slug: "alpine-gitlab-ruby-regex-incident"
date: 2026-10-29T01:02:37+0000
summary: "On October 20–21 an attacker tried to exploit CVE-2026-97229, a CVSS 10.0 regex bug in Ruby's Onigmo engine, against Alpine Linux's GitLab instance, which was a week behind on the fix. Alpine found no evidence the exploit succeeded but couldn't rule it out, so it treated the server as compromised, rebuilt it, and reset all passwords. Its checks show aports, packages and the October 21 releases were not tampered with, and signing keys were never on that server. The lesson for other operators: GitLab's fix is a patch to Ruby that only GitLab's own packages carry, so upgrading GitLab on a self-built image did not fix anything."
source: "https://alpinelinux.org/posts/GitLab-security-incident-CVE-2026-97229.html"
source_title: "GitLab security incident: attempted exploitation of CVE-2026-97229"
source_site: "Alpine Linux"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50282544"
---

Alpine's GitLab was attacked through a critical Ruby regex flaw, and it was close. A newly registered account sent crafted CI configurations to the instance on October 20 and 21. The instance still ran GitLab 19.3.4, because Alpine had upgraded to 19.3.4 instead of the fixed 19.3.5 by mistake. Eight GitLab worker crashes followed; in four of them the attacker got partway through the exploit and failed at the next step. The attacker also pulled Alpine's GitLab container image, which suggests they were tuning the exploit to that exact build.

Alpine found no evidence of code execution (no timing delays, unexpected files, keys, hooks or processes), but a successful exploit leaves no crash or web-log trace, some logs were gone, and outbound connections weren't logged. So it rebuilt the instance from scratch and reset all passwords. Users must set a new password when GitLab reopens, and should change it elsewhere if reused, since automated logins with leaked passwords were seen.

## Why upgrading wasn't enough

- CVE-2026-97229 (CVSS 10.0) is in Onigmo, the regex engine in Ruby, triggered by how GitLab evaluates regexes in CI configuration.
- GitLab's fix is a patch to the Ruby bundled in its own packages and images. Upstream Ruby shipped the fix on October 23, in 3.3.13, 3.4.12 and 4.0.9.
- Alpine's upgrade to 19.3.6 on October 22 didn't help, because its image used stock Ruby. It noticed on October 26. If you run GitLab on your own Ruby, you need one of those Ruby versions as well as the GitLab upgrade.
- The `alpinelinux/gitlab:19.3.6` image republished on October 27 contains Ruby 3.3.13. Earlier pulls and older tags are vulnerable.
- Any program that compiles regexes from untrusted input is exposed. Alpine's `ruby` package is fixed on all supported releases.

## What was and wasn't touched

- aports commits from October 20–22 were checked against a developer's clone and all 45 builders, and packages built October 20–24 matched. The October 21 releases (3.21.9, 3.22.7, 3.23.7, 3.24.3) were built from those verified commits. Checks cover up to October 24 and are being extended to October 27.
- Package and release signing keys have never been stored on the GitLab server.
- If the exploit had worked, exposure would include user emails and names, password hashes, 2FA secrets, CI/CD variables, runner tokens, integration credentials and private repositories.

## Changes Alpine is making

Apply critical GitLab releases within a day and check each for bundled-component patches, log outbound and database connections and keep logs longer than 14 days, and sign release tags so releases can be verified without GitLab.
