---
title: "Claude Code 2.1.313 extension on Cursor Marketplace shipped an SSH backdoor"
slug: "claude-code-cursor-extension-ssh-backdoor"
date: 2026-10-30T12:05:20+0000
summary: "Phorion found that version 2.1.313 of the official Claude Code extension on the Cursor Marketplace carried an injected payload that adds an attacker SSH key, opens a reverse SSH tunnel to an external server and installs persistence on macOS, Linux and Windows. It runs as soon as the extension loads, with no user interaction. Cursor has pulled the release, and the NPM and VS Code registries have also pulled 2.1.313. Anyone who ran that version should treat the machine as compromised and start incident response."
source: "https://phorion.io/blog/offical-claude-code-vsx-compromise/"
source_title: "Official Claude Code Extension Compromise: Security Advisory"
source_author: "Calum Hall"
source_site: "Phorion"
source_date: "2026-10-30"
hn_url: "https://news.ycombinator.com/item?id=50300683"
---

**Bottom line:** the Claude Code for VS Code extension, version 2.1.313, was backdoored on the Cursor Marketplace. The malicious code is prepended to the minified `extension.js` and runs when Node loads the file, before the extension activates. Cursor removed the release, and the 2.1.313 builds were also pulled from NPM and the VS Code registry.

## What the payload does

1. **Plants an SSH key.** It appends an attacker Ed25519 public key (comment `svc-telemetry`) to `~/.ssh/authorized_keys`, then tries to enable the local SSH server (systemctl on Linux, `sudo -n systemsetup` on macOS, OpenSSH Server via PowerShell on Windows).
2. **Opens a reverse tunnel.** It connects out to `134.209.82[.]225` with an embedded private key (`svc-telemetry-tun`) and forwards a dynamically allocated remote port back to the victim's port 22. The operator can then log in through the tunnel with the planted key. A retry loop keeps the tunnel alive after the IDE closes.
3. **Persists.** Linux uses `~/.config/.smt` plus an `@reboot` crontab entry. macOS uses a LaunchAgent disguised as `com.apple.sysmond.plist`. Windows uses a `SysMon` scheduled task or a Startup-folder script.
4. **Sends a recon beacon** over plain HTTP to port 8099 on the same server. It includes hostname, user, OS, git email, public IP and up to 40 environment variable *names* matching KEY/TOKEN/SECRET/PASS/AWS/GCP (not their values), which tells the operator which hosts hold cloud credentials.

## Bugs that limit the damage

On macOS, `systemsetup -getremotelogin` fails without admin rights but still exits 0. The script reads that error as "not Off" and never tries to turn Remote Login on. If SSH is off, the tunnel connects but nothing listens on port 22. Phorion stresses this is not a defence: if Remote Login is already on, or gets enabled later, the backdoor works. The Windows path also hardcodes `/bin/sh`, and the Linux crontab check can wipe existing user cron jobs on repeat runs.

## What to check

- Whether anyone ran Claude Code for VS Code 2.1.313 from the Cursor Marketplace.
- An `svc-telemetry` entry in `authorized_keys` (match by key fingerprint, since the comment is easy to change).
- `~/.config/.smt` and `.smtk`, the fake `com.apple.sysmond` LaunchAgent, a `SysMon` task, and `.stk_*` files in the temp directory.
- Outbound connections to the IP above.

Also discussed on HN with reports from other sources: the [Double Pulsar write-up](https://doublepulsar.com/anthropic-hacked-to-deliver-remote-access-trojan-to-claude-developer-laptops-worldwide-1148a92baae2) (blocked for this summary) and a [Mastodon thread](https://cyberplace.social/@GossiTheDog/117528831681803563).
