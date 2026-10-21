---
title: "Claude found 493 config and reachability issues in 25 OSS projects"
slug: "claude-attack-surface-reduction"
date: 2026-10-21T08:03:03+0000
summary: "Anthropic's Frontier Red Team used Claude to shrink the code attackers can reach, rather than hunt individual bugs. Across 25 open-source projects (5M+ lines), Claude reported 493 issues, mostly unnecessary pre-authentication code in default configs and config combinations that silently weaken security. Its biggest case was OpenSSH 10.4, where about 4,500 lines of experimental ML-DSA-44 code were reachable by unauthenticated clients; fixes shipped in 10.5 and 10.6."
source: "https://www.anthropic.com/research/attack-surface-reduction"
source_title: "Using Claude to reduce the attack surface of code"
source_author: "Chris Rohlf"
source_site: "Anthropic"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50175779"
---

**Bottom line:** Anthropic's Frontier Red Team argues that finding and fixing vulnerabilities one by one scales poorly, and that moving code behind a gate (authentication, user interaction, explicit opt-in) lowers the severity of bugs nobody has found yet. They call this initial research and used Claude to find that reachable code.

## Method

Claude looked for three things:

- **Default configuration:** code reachable in the shipped defaults that no default feature needs.
- **Configuration interaction:** option pairs that are fine alone but unsafe together, such as silently disabling a security control.
- **Early reachability:** complex or security-sensitive code that runs before authentication, user interaction or a protocol state change.

## Results

- 25 open-source projects (5M+ lines of C, C++, Java, PHP and Python); over 17,000 config options inventoried, 3,842 security-relevant. Claude reported 493 issues.
- All 11 "gated" projects had unintentionally reachable code: 93 unnecessary pre-auth surfaces in default configs, 37 places where code ran or state was recorded before authentication, and 61 config combinations that silently weakened a pre-auth control. None was immediately exploitable on its own.
- 162 security-sensitive option combinations were accepted without warning; 94 silently disabled a security control.
- Hostname-based deny rules failed open in four projects: with reverse DNS off, they never match.
- In one project, two options together silently dropped a required client-certificate check, a conflict that had existed for 21 years.

## OpenSSH case study

In a default OpenSSH 10.4 build, Claude found about 4,500 lines of experimental ML-DSA-44 code (roughly 16% of the daemon's pre-auth code) reachable by unauthenticated clients, even when the algorithm was excluded by configuration. After the report, 10.5 moved the check before parsing. Claude then found two remaining gaps (the check trusted the client-advertised algorithm name, and the certificate CA signature path wasn't gated). The 10.6 fix checks key types and CA signature algorithms against allow lists inside the parser, removing about a fifth of the pre-auth code reachable in a default build.

## Process and caveats

The techniques are packaged as a Claude skill, being integrated into OSS-Scanner, that runs three passes with fresh context each, confirms findings with a dynamic trace, and checks that default behavior is unchanged against the project's tests. Changes to non-default behavior are left to a human. The article gives no false-positive rate or count of how many of the 493 issues were confirmed or fixed, and reducing reachability makes bugs harder to reach rather than removing them.
