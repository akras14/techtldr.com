---
title: "Three commands put your macOS SSH key in the Secure Enclave behind Touch ID"
slug: "ssh-keys-secure-enclave-touch-id-macos"
date: 2026-10-21T17:05:00+0000
summary: "Nicolas Mattia shows how to create a non-extractable SSH identity in the Mac's Secure Enclave using the built-in sc_auth tool and Apple's ssh-keychain.dylib, so every SSH or git-over-SSH login asks for Touch ID and no private key file exists. The catch: the key dies with the machine, and the interface is largely undocumented."
source: "https://nmattia.com/posts/2026-10-19-setting-up-ssh-keys-touch-id/"
source_title: "Setting up SSH keys on macOS with Touch ID"
source_author: "Nicolas Mattia"
source_site: "nmattia.com"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50175882"
---

macOS can hold an SSH identity inside the Secure Enclave where it can't be extracted, and each use needs a fingerprint. No `~/.ssh/id_*` files to steal or delete.

- **Create it:** `sc_auth create-ctk-identity -l ssh-key -k p-256-ne -t bio`. `p-256-ne` is non-extractable and the only curve here that OpenSSH supports; `-t bio` requires Touch ID (`none` skips the prompt but the key still can't be extracted).
- **Load it:** `ssh-add -K -S /usr/lib/ssh-keychain.dylib`, and repeat after every reboot. An empty PIN at the prompt is fine, since security comes from Touch ID, not a PIN.
- **Register it:** `ssh-add -L` prints the `sk-ecdsa-sha2-nistp256...` public key to add to GitHub or any server. After that, `git push` triggers a Touch ID prompt.
- **How it works:** `ssh-keychain.dylib` is a FIDO-style security-key provider exposing `sk_load_resident_keys` and `sk_sign`; the agent calls them to list public keys and sign the server's challenge inside the enclave.
- **Caveats:** a dead Mac means a dead identity, so re-register elsewhere. The author derived this from experimentation and a community gist, notes much of it is undocumented, and didn't test multiple FIDO authenticators.
