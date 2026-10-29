---
title: "PHP2Shell chained with a kernel bug rooted 25 firewalls, including US government"
slug: "php2shell-chain-rooted-25-firewalls"
date: 2026-10-29T14:18:25+0000
summary: "Dream Research recovered the toolkit of a likely China-aligned campaign that chained a custom PHP2Shell exploit with the Fragnesia Linux kernel privilege escalation (CVE-2026-46300) to get unauthenticated root on a large vendor's firewalls in one HTTP request. Of about 19,000 internet-exposed devices found, 25 were rooted across the US, Japan, Taiwan and South Korea, and the attacker pivoted from two firewalls in a US government-affiliated network into its Active Directory. Dream sees signs of AI-assisted tooling but no proof the AI wrote the exploits."
source: "https://dreamgroup.com/blog/chained-php2shell-exploit-roots-25-servers"
source_title: "Chained PHP2Shell Exploit Roots 25 Servers, including US Government Servers"
source_site: "Dream Research"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50286987"
---
Dream Research found a complete attack toolkit on October 25: a custom, non-public PHP2Shell build (CVE-2026-109618/109619) chained with the Fragnesia kernel exploit (CVE-2026-46300), target lists, logs and about 40 post-exploitation scripts. It targeted specific versions of an undisclosed vendor's firewall software, whose management server runs PHP 8.0.30. Dream withheld the vendor and versions, and says the vendor has been notified.

## How the chain works

- **PHP2Shell:** abuses PHP's `session.upload_progress` in four stages (memory exhaustion, a pointer leak via Set-Cookie, a stale reference, then a forged HashTable whose destructor points at `system()`). This gives command execution as `nobody`.
- **Fragnesia:** SELinux confines that user, so the attacker used a kernel bug in XFRM ESP-in-TCP to overwrite `/etc/pam.d/su` through the page cache and make `su root` accept any password.
- Both run in one unauthenticated HTTPS request, and the full chain takes under 15 minutes. Exact memory offsets differ per firmware, so the attacker kept 11 version profiles, only one fully calibrated in a lab and the rest provisional.

## The campaign

1. About 19,000 exposed firewalls found via the FOFA search engine.
2. About 5,000 responded to version fingerprinting.
3. Filtered to the US, Japan, Taiwan and South Korea and to matching versions: 240 targets.
4. A batch runner rooted 24 and planted a 103-byte PHP webshell on each; 22 were then simply left parked.
5. Two firewalls in a US government-affiliated environment got a persistent reverse SSH tunnel, SOCKS proxy, and attacks on the firewall management server and Active Directory, with credential extraction and directory enumeration confirmed.

## AI angle

Claude Code was installed on the attacker's machine, an offensive platform called aiscan was configured with DeepSeek and Kimi keys, and the script style suggests AI-assisted development. Dream has no direct evidence AI wrote the exploits and says this isn't a fully autonomous attack. Its attribution to a nation-state is an inference from targeting, not proof. Dream is also selling an autonomous research product, which it used to find this.

Related: [PHP2Shell was exploited four days before disclosure](/posts/php2shell-exploited-four-days-before-disclosure/).
