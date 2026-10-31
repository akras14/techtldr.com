---
title: "A shared ChatGPT link could run attacker JavaScript as the victim"
slug: "chatgpt-share-link-account-takeover"
date: 2026-10-31T13:02:39+0000
summary: "Parameter's researchers chained a MathLive postMessage bug with a YouTube JSONP endpoint to escape ChatGPT's HTML Preview sandbox. Opening a malicious public share link ran attacker JavaScript on chatgpt.com in the victim's session. That let it read private chats and call authenticated APIs. OpenAI fixed it on October 26 and rated it P1."
source: "https://www.parameter.ai/labs/we-found-a-gap-at-openai"
source_title: "Do the Math: How We Took Over ChatGPT Accounts"
source_author: "Mukul Goyal"
source_site: "Parameter Labs"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50314699"
---

Parameter's researchers chained a MathLive postMessage bug with a YouTube JSONP endpoint to escape ChatGPT's HTML Preview sandbox. Opening a malicious public share link ran attacker JavaScript on chatgpt.com in the victim's session. That let it read private chats and call authenticated APIs. OpenAI fixed it on October 26 and rated it P1.

## How the chain worked

- **Sandbox:** Preview runs HTML on a separate origin, so it cannot read the main page directly. It can still send `postMessage` messages to it.
- **Listener:** A normal interactive graph loads MathLive, which installs a message listener in the main ChatGPT page. That listener accepted messages from any origin and let the sender replace the virtual keyboard layout with HTML.
- **HTML injection:** MathLive assigned that layout to `innerHTML`. A helper named `createHTML` only marked the string trusted and did not sanitize it. The researchers injected an `iframe` with `srcdoc` and no `sandbox` attribute, which inherited the `https://chatgpt.com` identity.
- **CSP bypass:** Inline script was blocked, but the CSP allowed YouTube. YouTube's oEmbed endpoint echoed a caller-supplied JSONP callback before returning its validation error. A callback ending in `;void` ran as script inside the new frame.

## Making it a one-link attack

The first version needed a researcher-controlled ChatGPT App. The final one put the graph and a later assistant-written HTML Preview in one ordinary conversation. The public `/share/<id>` link kept the whole chain. The victim needed one click on Preview, or none once the block was marked previewable. The authors reproduced the no-click path three times in a separate account.

The takeaway is that two harmless features, each isolated on its own, created a path across the sandbox. Third-party libraries in the main page, and any permissive allowlisted endpoints in the CSP, are what made the escape possible.
