---
title: "Private keys of eight eMudhra intermediate CAs compromised; update trust stores"
slug: "emudhra-intermediate-ca-keys-compromised"
date: 2026-10-20T15:02:54+0000
summary: "The private keys of eight eMudhra intermediate CA certificates, chained to roots trusted by Mozilla, Microsoft, Apple and Chrome, were shown to be compromised. Anyone holding a key could impersonate any website. Browsers have blocked the certificates, but clients that ignore revocation data need software updates."
source: "https://emudhra.we-have-a-problem.com/"
source_title: "eMudhra Intermediate CA Compromise"
source_author: "Dennis Jackson"
source_site: "we-have-a-problem.com"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50166579"
---

**Bottom line:** Eight eMudhra intermediate CA keys are compromised, so an attacker could impersonate any site to clients that still trust them. Browsers and OS vendors have already blocked them; the rest of the ecosystem needs updates.

- **Proof.** The original reporter ("Wayne") signed a message with each of the eight keys and reported it to the root programs on Oct 17. The site includes OpenSSL commands to verify.
- **Timeline.** Blocked in Firefox and Chrome Oct 17, Apple Oct 19, Microsoft Oct 20, via OneCRL, CRLite and CRLSets. The CA has also revoked the certificates.
- **Gap.** Clients that don't consume revocation data need software updates. Patched versions listed include Go 1.27.3 / 1.26.10, Electron 44.9.1 / 43.8.1 / 42.11.16, Python certifi 2026.10.19, rustls-webpki 0.103.16, webpki-roots 1.0.10, and NSS 3.131.1. Node.js and Composer ca-bundle are pending.
- **Linux workaround for experts.** Until distros ship updated ca-certificates, distrust the emSign ECC Root CA - G3 and Comodo AAA Services roots in the system store. The page has per-distro commands (Debian/Ubuntu, Alpine, Fedora family, SUSE, Arch, NixOS). It is not recommended for ordinary users.
- The page also has a client self-test and a JSON list of affected certificates with fingerprints.

**Also covered by:** [Scott Helme, "Eight Bits of Trust"](https://scotthelme.co.uk/eight-bits-of-trust-emudhras-ca-keys-cracked-in-milliseconds/) explains the cause: the P-384 private keys were tiny numbers (4, 56, 115 and so on, one byte each), so anyone could recover them from the public certificates in milliseconds, since April 2025. He notes that non-browser clients (curl, Python, Node, Go, CI runners, IoT) typically skip revocation and CT checks and are the real exposure.

**Also covered by:** [Mozilla Bugzilla bug 2083449](https://bugzilla.mozilla.org/show_bug.cgi?id=2083449) (the incident report, [HN discussion](https://news.ycombinator.com/item?id=50210067)). As of Oct 23, Mozilla says eMudhra has not answered the questions it sent on Oct 20, even after a 24-hour extension, and Chrome's root program called eMudhra's blaming of "social media scrutiny" unacceptable. The thread also records that the keys came from an Apr 30, 2025 key ceremony using Utimaco HSMs, with eMudhra attributing it to a code flaw in a wrap/unwrap test deviation. Commenters question how a certified HSM could produce one-byte private keys, and eMudhra has announced it is exiting the public TLS certificate business.

**Also covered by:** [captnemo, "Indian government sites are serving revoked eMudhra certificates"](https://captnemo.in/blog/2026/10/23/emsign-revoked-gov-in/) ([HN discussion](https://news.ycombinator.com/item?id=50218277)). Separately from the compromised keys, eMudhra revoked 9,356 certificates on its RSA hierarchy on Oct 19 (reason "Superseded"; they lacked a policy OID its own CP/CPS requires). As of Oct 23, 156 of 8,947 Indian government hosts checked still served revoked certificates, including the Controller of Certifying Authorities, which regulates eMudhra. Firefox (CRLite) refuses to load them, while Chrome's CRLSet lacks them, so Chrome, Edge and Android load them. The author also disputes the Indian IT ministry's claim that impact was limited and brief: the keys were weak from April 2025, and browsers are distrusting the whole G3 root, so about 30,000 certificates, mostly US universities via InCommon, must migrate.
