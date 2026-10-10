---
title: "Example.com adds six rotating languages, then drops the animation"
slug: "example-com-redesign"
date: 2026-10-10T21:31:50+0000
summary: "On 28 September, example.com got its biggest change in years: a JavaScript-driven page cycling its notice through six languages. Five days later the animation was gone and all six showed at once. IANA says the split into a bare HTML page plus a JS file cuts bandwidth, since most traffic is automated and never fetches the script, and repeats that the domain isn't for uptime testing."
source: "https://www.debugbear.com/blog/example-dot-com-redesign-history"
source_title: "Example.com Just Launched The Biggest Redesign In Decades"
source_author: "Conor McCarthy"
source_site: "DebugBear"
hn_url: "https://news.ycombinator.com/item?id=49971921"
---

On 28 September, example.com got its biggest change in years: a JavaScript-driven page cycling its notice through six languages. Five days later the animation was gone and all six showed at once. IANA says the split into a bare HTML page plus a JS file cuts bandwidth, since most traffic is automated and never fetches the script, and repeats that the domain isn't for uptime testing.

## The 2026 changes

- **28 September:** the static English page became multilingual (English, Arabic, Chinese, French, Russian and Spanish), switching language every five seconds. Each character sat in its own `span` with a slightly longer `transition-delay`, producing a character-by-character fade. The script also inserts an SVG book icon.
- **3 October:** the animation was removed, and all languages appear at once.
- The message now reads: "This domain is for use in documentation examples without needing permission. This is not a service, avoid relying on it for testing and monitoring purposes."

## IANA's explanation

Changes aim to reduce bandwidth or improve usefulness. The site is heavily trafficked, mostly by bots, so moving extra content into a separate JavaScript file reduces data served. IANA says it runs the HTTP site only as a courtesy; the domain's purpose is documentation, and it doesn't need a web server at all.

## 20-plus years of history

- **1999:** the site launches. The earliest Wayback copy (January 2002) is a table-layout list of reserved domains with the ICANN logo, served by Apache 1.3.22.
- **March 2002:** the familiar "reserved for use in documentation" wording appears. **2003:** a link to RFC 2606 is added.
- **2010:** example.edu joins the list. From 2011 the domain redirected to IANA's site.
- **July 2013:** its own page returns, served from the EdgeCast CDN, with the rounded "Example Domain" card.
- **2019:** font stack and copy tweaks. **January 2025:** the Server header disappears after EdgeCast/Edgio's troubles. **October 2025:** the card is dropped and the HTML minified. **December 2025:** Cloudflare starts serving it.
- **June 2026:** an empty favicon (`<link rel="icon" href="data:," />`) stops browsers requesting /favicon.ico.

DebugBear's own uptime monitoring shows example.com occasionally fails to respond correctly, so use infrastructure you control for connectivity checks.
