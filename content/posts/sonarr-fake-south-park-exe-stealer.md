---
title: "A fake South Park .exe with 2,392 seeders was a 887 MB info-stealer"
slug: "sonarr-fake-south-park-exe-stealer"
date: 2026-10-15T20:02:14+0000
summary: "A Sonarr user found a torrent posing as a new South Park episode that was actually a Windows .exe: a 118 KB loader padded with about 886 MB of junk to dodge scanners, which decrypts an in-memory ChaCha20 payload. The payload is an info-stealer targeting browsers, crypto wallets, password managers, SSH and cloud credentials, and it finds its server via a YouTube channel and a GitHub gist."
source: "https://thereallo.dev/blog/someone-on-torrent-sites-kept-fucking-with-my-sonarr-setup"
source_title: "Someone on Torrent Sites Kept Fucking With My Sonarr"
source_author: "Thereallo"
source_site: "thereallo.dev"
source_date: "2026-10-11"
hn_url: "https://news.ycombinator.com/item?id=50108872"
---

**Bottom line:** Sonarr grabbed a "South Park S29E03" torrent that was really an .exe, and the author reverse-engineered it. Sonarr can't execute it, so the risk is a person double-clicking it. The fix takes minutes: have qBittorrent exclude executable file names, and blocklist with "Remove from client" in Sonarr.

**The sample:**
- A real x64 PE where the actual program is 118 KB and the other ~886 MB is random overlay, likely to make scanners give up on large files.
- The loader is full of no-op XOR junk to garble decompiler output. Its real logic is six steps: read the last 64 bytes of itself, derive a key, decrypt a hidden payload with ChaCha20, map it into memory manually, repoint the PEB image base, and jump in. Nothing touches disk. Flags can enable anti-debug and anti-sandbox checks.
- The key sits in the file itself, so the payload can be decrypted statically with a short Python script.

**The payload:** A 929 KB C++ stealer with modules for browsers (including Chrome app-bound encryption), crypto wallets, password managers, Telegram/Discord/Steam, SSH, AWS/Azure/gcloud/kube/docker configs, remote-access tools, email, VPN configs and desktop documents. Strings are AES-128-CTR encrypted per string; the author emulated the call sites in Unicorn and recovered 601 of 672.

**Command and control:** The address is fetched from a YouTube channel and a GitHub gist (XOR plus base64), with the same domain also hardcoded as a fallback. The domain was registered September 14, builds date to October 6, and the torrent appeared October 8.

**Mitigations:** In qBittorrent, exclude `*.exe`, `*.scr`, `*.lnk`, `*.bat` and `*.cmd`. Blocklist with removal in Sonarr/Radarr, and demote trackers that keep serving this.
