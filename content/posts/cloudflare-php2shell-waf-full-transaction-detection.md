---
title: "Cloudflare shipped php2shell WAF rules 9 hours early, using request-plus-response detection"
slug: "cloudflare-php2shell-waf-full-transaction-detection"
date: 2026-10-21T17:04:00+0000
summary: "Cloudflare deployed default-on WAF protection against php2shell (CVE-2026-109618 and CVE-2026-109619) for all plans, including Free, at 03:02 UTC on October 21, about 9 hours before disclosure. The rules use a new capability, full transaction detection, which inspects the origin's response as well as the request. Cloudflare saw attackers mutating payloads after the leak and says patching PHP is still required."
source: "https://blog.cloudflare.com/php-vulnerabilities-waf-protection/"
source_title: "Cloudflare deploys protections against php2shell vulnerability"
source_author: "Cloudflare"
source_site: "The Cloudflare Blog"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50181326"
---

Cloudflare turned on managed WAF rules for [php2shell](/php2shell-rce-every-php-version/) for all customers, with no action needed on the Free plan. Paid plans get them in the Cloudflare Managed Ruleset, which should be in block mode. Cloudflare worked with the PHP Foundation, OpenAI and Wiz and had mitigations out about 9 hours ahead of public disclosure.

- **Why request rules weren't enough:** request data alone often can't separate an exploit from legitimate traffic. Cloudflare built full transaction detection, which combines request and response signals in one HTTP transaction, so a matching origin response can be blocked before it reaches the requester. php2shell is the first managed detection to use it.
- **Attacks seen:** the bugs leaked before the official release, and Cloudflare saw attack traffic against customers, with attackers mutating payloads to evade rules. It tracked match volume against rule logic to adapt.
- **Bug summary:** both CVEs involve PHP mishandling memory after an error such as running out of memory. 109618 uses data not fully initialized; 109619 follows stale references into freed memory on a later request. The fix has to be in the PHP runtime, not application code or plugins.
- **Advice:** patch PHP anyway and treat the WAF as defense in depth. Limiting uploads and upload sizes reduces exposure but doesn't replace the fix. The post lists the rule IDs for Free and paid rulesets.
