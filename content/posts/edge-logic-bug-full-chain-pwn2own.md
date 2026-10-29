---
title: "Five logic bugs chain into Edge code execution, no memory corruption"
slug: "edge-logic-bug-full-chain-pwn2own"
date: 2026-10-29T09:02:54+0000
summary: "Orange Tsai's Pwn2Own Berlin 2026 Microsoft Edge exploit (which won a Pwnie for Epic Achievement) chains five logic bugs and two Windows features into zero-click code execution, with no memory corruption and no AI. It starts with a UXSS: Edge's EdgeCAGuidanceNavigationThrottle launches a browser-initiated navigation with no source tab, so Chromium applies it to whichever tab is active. The chain then escapes the sandbox through an arbitrary file write in the Immersive Reader WebUI. Tsai spent two months on the research and says finding the bugs was the hard part, while chaining them was easy."
source: "https://blog.orange.tw/posts/2026-10-the-last-proof-of-human-craft-browser-full-chain/"
source_title: "The Last Proof of Human Craft: Forging a Microsoft Edge Full Chain with Logic Bugs Only!"
source_author: "Orange Tsai"
source_site: "blog.orange.tw"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50285319"
---

**Bottom line:** Orange Tsai (DEVCORE) built a completely handmade Microsoft Edge exploit for Pwn2Own Berlin 2026 (a $175,000 win) out of logic bugs only. It took two months and used no memory corruption and no AI, and it won a Pwnie for Epic Achievement.

**The chain**

1. **Sourceless navigation (UXSS).** Edge's `EdgeCAGuidanceNavigationThrottle` takes a `switch_profile` URL from the query string and starts a new navigation from the browser process. The navigation passes no source tab, so Chromium's `Navigate()` falls back to the currently active tab. If the victim switches tabs after the throttle fires, attacker JavaScript runs on any site, such as google.com. The browser-initiated navigation also ignores CSP.
2. **Making it zero-click:**
   - The throttle checks `GetLastCommittedURL()` for a Microsoft login origin, but a popup navigated from another window passes the check.
   - A hardcoded popup-blocker allowlist entry (`about:blank#quickAuthPopup`) lets the popup open.
   - The OneAuth feature signs the user into an Edge profile automatically.
   - A CORS misconfiguration on `assets.msn.com` leaks the signed-in user's email to any website.
3. **Sandbox escape.** Chromium refuses JavaScript URLs on privileged WebUI schemes, but Edge's forked Immersive Reader uses a `read://` scheme that is missing from the blocklist. From there, `edgeFeedbackPrivate.zipDiagnosticLogFiles()` allows path traversal, and a NULL byte removes the forced `.json` extension, giving an arbitrary file write.
4. **Code execution.** The file write overwrites `protocols.json` in Edge's AutoLaunchProtocolsComponent, so any origin can launch any protocol handler. A navigation to `edge://restart` reloads it. Launching `telnet://` makes Windows search PATH for `telnet.exe`, and one PATH directory is writable by a low-privilege user, so a small dropped 32-bit PE runs. The whole exploit is under 100 lines.

**Takeaways:** Tsai considers Chromium navigation throttles a neglected attack surface (fewer than 20 of 3,000+ Chrome CVEs in five years) and sees the sourceless-navigation UXSS as a new primitive. With logic bugs, finding them is the hard part and chaining is easy. He kept this chain AI-free partly to explore the surface before handing it to AI labs, though he says he uses coding agents heavily elsewhere.
