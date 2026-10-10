---
title: "Telegram Desktop one-click account takeover via IPC injection"
slug: "telegram-desktop-ipc-injection"
date: 2026-10-10T05:02:38+0000
summary: "A researcher found that Telegram Desktop (through 7.2.8) fails to escape the semicolon separator in the messages it passes between its own processes, so one clicked link can inject extra commands and, combined with a missing authorization check, read arbitrary files from the victim's disk and send them to the attacker. Stolen session files are enough to take over the account. It is fixed in 7.2.9 (CVE-2026-107181, CVSS 8.1)."
source: "https://beaksec.github.io/posts/telegram-desktop-one-click-account-takeover/"
source_title: "Telegram Desktop: one-click account takeover via IPC injection"
source_author: "beaksec"
source_site: "beaksec"
source_date: "2026-10-03"
---

**Bottom line:** Telegram Desktop through 7.2.8 can be made to hand over its login session after a single link click. Two flaws combine: an injection bug in how a second app launch talks to the running one, and an internal command that reads any file and sends it to a chat with no authorization check. Upgrade to 7.2.9 or later.

## How it works

- When a `tg://` link is clicked while Telegram is running, the new process forwards the URL to the running instance over a local socket as text. Each instruction ends in a semicolon (`OPEN:<url>;`), and the semicolon is never escaped.
- A URL containing `;OPEN:...` therefore becomes several commands. `OPEN:` accepts any URL scheme, including `interpret:`, an internal scheme once used to publish Telegram's own releases.
- `interpret:` reads an instruction file naming a file and a destination channel, then uploads that file with no confirmation or check on who asked. That was harmless from the command line, but reachable through the socket it becomes arbitrary file read.

## The attack chain

- The attacker adds the victim to a supergroup (allowed by default) and posts small text files. Default auto-download saves them to the Downloads folder under predictable names. Relative paths remove the need to know the user name.
- Stacking several injected `OPEN:interpret:` commands in one link exfiltrates several files. The targets are `tdata/key_datas` and two other files. Without a local passcode, the key salt is stored next to the encrypted key, so these files decrypt the session and let the attacker open the account elsewhere.
- The link must be clicked outside Telegram, so the attacker sends a normal https link that redirects to the crafted `tg://` URL. The browser or OS may show a launch prompt.

## Mitigations

Upgrading is the only real fix. Otherwise, enable "ask where to save each file" (stops the instruction file landing on disk), limit who can add you to groups, and set a local passcode. A passcode does not prevent theft but makes the stolen data harder to use. Confirmed on Windows.
