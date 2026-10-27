---
title: "GoAhead 6.1.0 accepts revoked client certs: three TLS bugs"
slug: "goahead-tls-bugs-revoked-certs-accepted"
date: 2026-10-27T20:02:31+0000
summary: "GoAhead, an embedded web server used in hundreds of millions of routers, firewalls, printers and industrial devices, has three independent TLS bugs through version 6.1.0, fixed in 6.1.1. The worst (CVSS 9.1) is that the OpenSSL backend loads a configured CRL but never turns on revocation checking, so revoked client certificates still pass mutual TLS."
source: "https://foxio.io/blog/choose-your-backend-three-tls-bugs-in-goahead"
source_title: "Choose Your Backend: Three TLS Bugs in GoAhead"
source_author: "Sébastien Féry"
source_site: "FoxIO"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50263701"
---

GoAhead, an embedded web server used for device management in routers, firewalls, printers and industrial controls, has three independent TLS bugs in versions up to 6.1.0. All are fixed in 6.1.1. Which one affects a device depends on the TLS library and verification settings compiled into it.

- **Revoked certificates accepted (OpenSSL-compatible backend, critical, CVSS 9.1).** GoAhead loads the configured CRL into the certificate store but never sets `X509_V_FLAG_CRL_CHECK`, so OpenSSL skips revocation checking entirely. On GoAhead 6.1.0 with OpenSSL 3.6.2, a revoked client certificate completed the mutual-TLS handshake, and startup gives no warning. It affects 3.4.5 through 6.1.0 with a CRL configured. The fix is one added line setting the flag.
- **Broken Diffie-Hellman modulus (Mbed TLS backend, medium, CVSS 5.9).** The code passes `sizeof(dhm_g)` (1 byte) as the length of the 256-byte prime, so the server sends a one-byte DH modulus of 255. That leaves eight possible shared secrets, and a passive observer can decrypt captured TLS 1.2 DHE_RSA sessions. It needs a client that accepts such parameters; the tested OpenSSL client rejected them. The typo dates to 2018 and affects 4.0.2 through 6.1.0. ECDHE and TLS 1.3 are unaffected.
- **Chain-depth limit bypass (LibreSSL legacy verifier, high, CVSS 7.4).** With client certificates required but issuer verification off, an over-depth client chain is accepted. This applies to LibreSSL 3.2.x and 3.3.2–3.3.6.

The post lists the affected configurations and mitigations. For the OpenSSL bug, upgrade to 6.1.1. For the Mbed TLS bug, upgrade, disable finite-field DHE suites, or switch to the OpenSSL backend. For the LibreSSL bug, enable `VERIFY_ISSUER=1` with a trusted CA bundle. It also gives a way to spot exposure in JA4S logs by looking for DHE_RSA cipher suites.
