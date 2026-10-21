---
title: "Three commands to put your SSH key in the Mac Secure Enclave"
slug: "ssh-keys-secure-enclave-touch-id-macos"
date: 2026-10-21T07:02:51+0000
summary: "macOS can hold an SSH identity inside the Secure Enclave, where it can't be extracted and every use requires Touch ID. Setup is three commands using Apple's built-in sc_auth and ssh-keychain.dylib, and it works for git over SSH including GitHub. The catch: the key dies with the machine, and you must re-add it to the agent after each reboot."
source: "https://nmattia.com/posts/2026-10-19-setting-up-ssh-keys-touch-id/"
source_title: "Setting up SSH keys on macOS with Touch ID"
source_author: "Nicolas Mattia"
source_site: "nmattia.com"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50175882"
---

**Bottom line:** macOS can generate an SSH identity inside the Secure Enclave that can't be extracted, never exists as a key file, and prompts for Touch ID on every use. It takes three commands and works for git over SSH, including GitHub.

## Setup

1. Create the identity (once): `sc_auth create-ctk-identity -l ssh-key -k p-256-ne -t bio`
2. Tell SSH to use it (after every reboot, press enter at the PIN prompt): `ssh-add -K -S /usr/lib/ssh-keychain.dylib`
3. Get the public key to register on a server: `ssh-add -L`, and use the line starting with `sk-ecdsa-sha2-nistp256`.

After that, `git push` triggers a Touch ID prompt.

## Details worth knowing

- `p-256-ne` is the only key type that works here: the `-ne` suffix means non-extractable, and OpenSSH doesn't support the p-384 option.
- `-t bio` requires biometrics per use. `-t none` still prevents extraction but lets programs sign silently, which avoids prompts on every `git pull`.
- Apple's `ssh-keychain.dylib` acts as a translation layer, so ssh-add treats the enclave identity like a FIDO-style authenticator.
- Much of this is undocumented; the author pieced it together from other write-ups, man pages and experiments.

## Trade-offs

The key can't be backed up or moved, so if the Mac dies you create a new identity and re-register it everywhere. The agent also forgets the identity on reboot.
