---
title: "Passkeys on a Mac require iCloud Keychain, so he sticks with passwords"
slug: "passkeys-icloud-keychain-backups"
date: 2026-10-16T19:03:05+0000
summary: "Apple developer Jeff Johnson explains why he still avoids passkeys, independent of the attestation controversy: macOS only saves a passkey locally if iCloud Keychain is on, and he neither trusts iCloud nor can make reliable backups of it. He wants to log in from a device that's new to him without signing it into a cloud service, and to keep redundant backups, both of which passwords satisfy."
source: "https://lapcatsoftware.com/articles/2026/10/6.html"
source_title: "Why I still shun passkeys"
source_author: "Jeff Johnson"
source_site: "lapcatsoftware.com"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50119328"
---

**Bottom line:** Jeff Johnson says passkeys still feel like a trap to him, for reasons that have nothing to do with attestation: on Apple platforms they are tied to iCloud Keychain, which he distrusts and which is hostile to backups. He keeps using random per-site passwords. He allows his reasons may be specific to the Apple ecosystem.

- **Two requirements.** He must be able to log into a site from a browser on a device that's new to him (a friend's, a library's) without signing that device into a cloud account, and he must be able to make reliable, redundant backups of his credentials. Passwords can be read off one screen and typed into another, written down or memorized.
- **Why passkeys fall short.** A YubiKey can't be backed up and is inconvenient as the primary store. On macOS, saving a passkey locally requires iCloud Keychain; the other options store it on a different device or security key. He doesn't sync personal data to iCloud, and says the old login keychain was a single copyable file but that, as of macOS 26.4, this is no longer true.
- **Credential exchange doesn't help him.** The new protocol for moving passkeys between managers addresses migration, which he doesn't need, and he doesn't want to depend on third-party credential managers.
- **His ask.** Let users save passkeys in a local, back-up-able format, or at least manage the passkey database manually. He argues this isn't just a power-user concern, since the backup problem hurts less sophisticated users too, who then ask sophisticated ones for help.

See also [Scott Helme's rebuttal](/posts/passkeys-not-a-trap-rebuttal/) of the related attestation claims.
