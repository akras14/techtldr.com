---
title: "Post-quantum cryptographic right answers: ML-KEM key exchange, ML-DSA-44 signatures"
slug: "pq-cryptographic-right-answers"
date: 2026-10-12T18:02:58+0000
summary: "Filippo Valsorda updates the \"Cryptographic Right Answers\" list for the post-quantum era. The post-quantum transition now has a consensus deadline of 2029, so key exchange should be X25519+ML-KEM-768 and signatures ML-DSA-44, while symmetric crypto stays largely as it was. He also adds that a review from the best affordable LLM is now a right answer."
source: "https://words.filippo.io/right-answers/"
source_title: "(Post-Quantum) Cryptographic Right Answers"
source_author: "Filippo Valsorda"
source_site: "words.filippo.io"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50055409"
---

**Bottom line:** Extending the series begun by Colin Percival (2009), Thomas Ptacek (2015) and Latacora (2018, 2024), Filippo Valsorda says the post-quantum timeline shifted in early 2026 and there is now consensus that the transition must finish by 2029. Symmetric cryptography is fine, because Grover's algorithm doesn't matter in practice, but every asymmetric answer changed. He also says LLMs are now very good at finding bugs, so "get a review from the best model you can afford" joins "use a memory-safe language" as a right answer.

**His cheat sheet (assuming no strict compliance regime)**
- **Encrypting data:** use the cloud KMS; otherwise XAES-256-GCM or XChaCha20Poly1305 with random nonces. For more than a few MB, use Cobblestone-128, a STREAM variant. Never pass one huge message to an AEAD.
- **Symmetric key length:** 256-bit if you must pick. **Symmetric signatures:** HMAC-SHA-256, over a verbatim byte string.
- **Hashing:** SHA-2 is fine (SHA-512/256 avoids length extension). Use SequenceHash or TupleHash when hashing multiple values.
- **Random IDs:** 32 random bytes from the OS CSPRNG.
- **Passwords:** Argon2id or scrypt, or consider passkeys.
- **Key exchange:** X25519+ML-KEM-768 inside a standard protocol.
- **Asymmetric encryption:** HPKE with X-Wing (MLKEM768-X25519), HKDF-SHA256 and AES-128-GCM, or age with PQ keys.
- **Signatures:** ML-DSA-44.
- **TLS:** cloud ALB/NLB, or rustls, Go's crypto/tls, BoringSSL, AWS-LC or s2n-tls with an updated ACME client.
- **Backups:** restic with a long random password.

I summarized the cheat sheet and the sections on symmetric cryptography, hashing, IDs and passwords; the post's deeper sections on the asymmetric choices are not covered here.
