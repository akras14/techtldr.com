---
title: "Synced passkeys send no attestation, so the 'passkeys are a trap' claim fails"
slug: "passkeys-not-a-trap-rebuttal"
date: 2026-10-14T16:02:14+0000
summary: "Security researcher Scott Helme rebuts the viral post telling people to drop passkeys for TOTP and passwords. Synced passkeys (iCloud Keychain, Google Password Manager, 1Password, Bitwarden) return no attestation at all, and consumer sites have been advised for years not to request it, so a site demanding it would lock out nearly all its users. Where attestation exists it only identifies the authenticator's make and model, not your OS, browser or extensions. Helme also notes that TOTP is phishable by relay kits like Evilginx, while passkeys are bound to the real domain, so his advice is to keep using them."
source: "https://scotthelme.co.uk/passkeys-are-not-a-trap/"
source_title: "Passkeys Are Not a Trap"
source_author: "Scott Helme"
source_site: "scotthelme.co.uk"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50088935"
---

**Bottom line:** Scott Helme, who built and open-sourced a passkeys library, argues that the viral "Passkeys are a trap. Do not use them." post misdescribes how passkeys work, and that following its advice leaves you more exposed to phishing. This is a direct response to [our earlier summary](/posts/passkeys-attestation-lock-in/) of that post.

## The rebuttal

- **Synced passkeys don't attest.** Passkeys stored in iCloud Keychain, Google Password Manager, 1Password or Bitwarden give sites no attestation statement; a site can ask, but the answer is "none". A consumer site that required attestation would lock out almost all its own users, and guidance for consumer sites has long been not to request it.
- **Attestation isn't what the original claimed.** When present, it reveals the authenticator's make and model (its AAGUID) with a vendor certificate chain. It says nothing about your OS, browser, extensions, rooted or jailbroken state, or firmware.
- **The YubiKey advice backfires.** The original recommends hardware keys because they "avoid OS-wide attestation", but YubiKeys ship with Yubico attestation certificates, and enterprise attestation exists so organizations can require approved hardware.
- **TOTP is a step backwards.** Codes can be typed into the wrong site, and relay kits such as Evilginx capture the password and code in real time and take the session. A passkey is bound to its domain, so the browser won't offer it on a lookalike site. Helme notes the original concedes this in an addendum.

## What he recommends

Keep using passkeys wherever offered, in a manager you trust, ideally one that supports exporting credentials. If attestation ever creeps into consumer logins, push back then; it hasn't happened, and avoiding passkeys now only leaves you open to phishing.
