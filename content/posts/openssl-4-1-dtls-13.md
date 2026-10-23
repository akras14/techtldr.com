---
title: "OpenSSL 4.1.0 adds DTLS 1.3 and GREASE, drops Itanium and Windows CE"
slug: "openssl-4-1-dtls-13"
date: 2026-10-23T14:04:42+0000
summary: "OpenSSL 4.1.0, released 23 Oct 2026, is a feature release headlined by DTLS 1.3 (RFC 9147) support. It also adds RFC 8701 GREASE, DTLS in the SSL listener API, an IKEv2 KDF and initial Elbrus2000 support, with several post-quantum and AES speedups on x86_64 and ppc64le."
source: "https://github.com/openssl/openssl/releases/tag/openssl-4.1.0"
source_title: "OpenSSL 4.1.0"
source_author: "OpenSSL project"
source_site: "github.com"
source_date: "2026-10-23"
hn_url: "https://news.ycombinator.com/item?id=50208099"
---

**Bottom line:** OpenSSL 4.1.0 is a feature release whose main addition is DTLS 1.3 ([RFC 9147](https://www.rfc-editor.org/rfc/rfc9147)). It also adds some breaking build and tooling changes to check before upgrading.

**New features**

- DTLS 1.3 support (see the `ossl-guide-dtlsv13(7)` man page), plus DTLS support in the SSL listener API.
- [RFC 8701](https://www.rfc-editor.org/rfc/rfc8701) GREASE, which sends random extension values to keep peers tolerant of future extensions.
- An IKEv2 KDF, and initial support for the Elbrus2000 (`e2k`) architecture.
- Performance work: optimized ML-DSA and ML-KEM NTT operations on `ppc64le`, optimized ML-DSA on `x86_64` (including AVX-512 SHAKE x4), and AVX-512/VAES speedups for AES-CBC decryption on `x86_64`.

**Things that may break**

- `tsget` now needs `Net::Curl::Easy` from CPAN instead of the abandoned `WWW::Curl::Easy`.
- The `no-ecdsa` and `no-ecdh` Configure options are gone because they never really disabled anything; use `no-ec`.
- The Windows-on-Itanium and Windows CE targets are dropped, and new MSVC 2013 build targets bridge missing C99 support.

**Known issues in 4.1.0:** a DTLS 1.3 KeyUpdate drops other outstanding post-handshake records (such as a NewSessionTicket) from the retransmission buffer, so a lost original is not resent. Separately, error reporting on DTLS listeners and accepted connections can be wrong: a permanent receive failure may show up as `SSL_ERROR_WANT_READ`, and a retryable write failure as `SSL_ERROR_SYSCALL`. A fix for the first is in progress.
