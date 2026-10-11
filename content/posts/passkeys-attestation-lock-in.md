---
title: "Passkeys can be locked to approved software through attestation"
slug: "passkeys-attestation-lock-in"
date: 2026-10-11T23:05:00+0000
summary: "A blogger argues that passkeys, unlike passwords or TOTP codes, give up user control because the WebAuthn standard lets a site demand attested passkeys. Attestation proves your passkey manager, OS and firmware are on an approved list, so a site could one day reject custom software, Linux or a de-Googled phone. Attestation is not required on most sites yet, but the author expects constraints to be added gradually and recommends sticking with passwords and TOTP."
source: "https://nonfunctional.substack.com/p/passkeys-are-a-trap-do-not-use-them"
source_title: "Passkeys are a trap. Do not use them."
source_author: "The Hack"
source_site: "Non-Functional"
source_date: "2026-10-10"
hn_url: "https://news.ycombinator.com/item?id=50048312"
---

**Bottom line:** the author's case is that passkeys look like a convenient, more secure replacement for passwords, but the standard includes a mechanism, remote attestation, that lets websites decide which software and hardware you may use. Today most sites don't enforce it. The author expects that to change gradually.

**Why not just call them "passkeys":** the post argues the FIDO Alliance gave one name to many different things. A passkey can be copyable or not, transferable or not, hardware-backed (secure chip) or a plain file, and require user presence and verification or not. The WebAuthn spec itself barely uses the word.

**The catch:** passkey software must report these properties to the site. You can write your own software that lies about them, except for *attested* passkeys. Attestation uses the device's tamper-resistant chip to prove that your passkey manager, OS and firmware are on an approved list. A site that requires it can reject a self-built passkey manager, Linux, or a phone running LineageOS or GrapheneOS.

**The argument about motives:** the FIDO Alliance includes big tech, banks and intelligence agencies, but no consumer advocacy groups. The author suspects attestation is a way to stop people running unapproved software, such as alternative messengers or devices without mandatory scanning, not just a way to stop password sharing.

**What the author prefers:** passwords are insecure but you control them. TOTP authenticator codes (RFC 6238) already solve the security problem, work with any software or hardware, and can be stored or shared as you choose. Passkeys are fine if you know which kind you are creating, but the author would avoid them.

The author admits attestation has legitimate uses, such as rejecting a scammer's fake passkey app, and that most people on the big four operating systems will never fail a check.
