---
title: "MuPDF's AES-256 PDF keys have only 9 bits of entropy"
slug: "pdf-aes-256-weak-keys"
date: 2026-10-28T23:04:14+0000
summary: "Several PDF libraries that advertise AES-256 encryption generate the file key from tiny or predictable state, so a protected PDF can be decrypted without the password. In MuPDF before 1.28.5, only 9 bits of the timestamp-seeded RNG affect the key, so all 512 candidates can be tried in about 11 ms. Foxit PDF Reader derives it from a SHA-256 of the current time, and OpenPDF and iText seed an ARC4 stream with time and free JVM memory."
source: "https://v12.sh/blog/the-case-of-the-9bit-aes-key"
source_title: "The case of the 9 bit AES key"
source_author: "Rick de Jager"
source_site: "V12"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50280722"
---

**Bottom line:** Several PDF tools that claim AES-256 encryption build the random file key from state small enough to enumerate. Password-protected PDFs made with them can be opened without the password. MuPDF was fixed in 1.28.5; already-published files stay weak.

**How PDF encryption works:** The password does not encrypt the document. The writer generates a random 32-byte file key, encrypts the content with it, and then encrypts that key under a key derived from the password. If the file key is predictable, the password is irrelevant.

**MuPDF (before 1.28.5):** It uses a drand48-style 48-bit LCG seeded once with `time(NULL)`. Each output byte comes from bits 17 to 24 of the state, and high bits never affect low bits. The low 16 bits are a constant, so only 9 bits of the timestamp matter. Seeds 512 seconds apart give identical streams. Brute-forcing all 512 candidates took about 11 ms in unoptimized Python, and the plaintext `/ID` in the trailer, drawn from the same generator, gives a check.

**Foxit PDF Reader (2026.1.3):** The key is SHA-256 of the 4-byte timestamp, 32 uninitialized bytes (in practice zeros) and the constant "there". The `/Perms` block holds known plaintext, so each guess costs one hash and one AES block. A Rust search on a Ryzen 9 7950X ran at about 370 million timestamps per second. It covered every second since 1970 in 4.85 s, and a recent file takes milliseconds. The same pattern appears in PDFium's first public commit (2014), credited to Foxit, so other editors may have inherited it.

**OpenPDF 3.0.5 and iText 9.7.1:** Randomness comes from an ARC4 stream seeded with the millisecond time and the JVM's free memory. There is more variance than in the other two, but the plaintext salts in `/U` and `/O` let an attacker check guesses.

The authors disclosed the issues to the vendors. They note that the "upgrade" to AES-256 left some files weaker than the older password-derived keys.
