---
title: "PHP's OOM remote-code-execution bug was known in 2024; the fix stalled until August"
slug: "php-foundation-oom-fix-stalled-two-years"
date: 2026-10-21T17:03:00+0000
summary: "The PHP Foundation says the out-of-memory bug behind CVE-2026-109618 was spotted in a 2024 audit but its fix stalled for months because every candidate approach had trade-offs and the consensus-driven security team couldn't choose. It became top priority only after Calif, OpenAI and Wiz showed unconditional RCE in August 2026. The post also covers a leak two days before release, a Chainguard embargo break, and a funding appeal."
source: "https://thephp.foundation/blog/2026/10/21/todays-php-security-release-and-how-were-strengthening-php-security/"
source_title: "Today's PHP Security Release, and How We're Strengthening PHP Security"
source_author: "The PHP Foundation"
source_site: "The PHP Foundation"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50182987"
---

This is the PHP Foundation's account of the [October 21 security release](/php-critical-security-release-oct-21/) (PHP 8.5.12, 8.4.27, 8.3.36, 8.2.35), which fixed two critical RCE bugs. The Foundation admits the delay was "a structural failure": a fix for the underlying out-of-memory problem stalled for months.

- **CVE-2026-109618:** if a request hits `memory_limit` at the wrong moment, PHP leaves partly initialized data behind and later treats it as valid. On Apache with mod_php, RCE was shown with default settings, before any application code runs.
- **CVE-2026-109619:** a fatal error leaves state pointing at memory from a previous request, so a later request on the same worker can use freed memory.
- **Why the fix stalled:** a 2024 audit by Quarkslab (funded by the Sovereign Tech Agency) surfaced the first signs of the OOM problem. The security team drafted competing fixes, each with backward-compatibility breaks, new DoS potential or required third-party extension changes, and couldn't agree. With no known RCE path, it kept restarting.
- **What changed it:** on August 11, 2026 Calif reported unconditional RCE with the same root cause, and OpenAI independently reported it the same day. Wiz and OpenAI added further vectors. Fixing it then became the team's top priority.
- **Process changes:** an Ecosystem Security Team (May 2026), new hires, majority-vote decisions, and AI tooling (Claude Mythos, OpenAI's Daybreak Red) for scanning and validating reports. Fifteen CVEs have been published since July 1, and over 100 reports were invalid or duplicates. A human makes every call.
- **Leak and embargo:** the CVE IDs leaked on October 20 with a wrong CVSS (10.0 vs. the real 9.2), so the pre-announcement and release were each pulled forward. Calif accidentally published a PoC video. Chainguard published fixed images over six hours before the coordinated release.
- **The ask:** sponsorship from companies, hosts and governments that depend on PHP, since tools find bugs faster but can't decide engine-level compatibility trade-offs.
