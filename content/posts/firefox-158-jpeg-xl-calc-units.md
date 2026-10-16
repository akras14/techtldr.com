---
title: "Firefox 158 turns on JPEG XL by default and adds typed arithmetic in CSS calc()"
slug: "firefox-158-jpeg-xl-calc-units"
date: 2026-10-16T19:03:05+0000
summary: "Firefox 158 (released October 13, 2026) enables JPEG XL image support, which Mozilla says generally compresses better than WebP, JPEG, PNG and GIF. It also adds typed arithmetic in CSS calc() so units can be multiplied and divided, more accessible PDFs when saving or printing a page, and smoother scrolling without rounding to whole pixels."
source: "https://www.firefox.com/en-US/firefox/158.0/releasenotes/"
source_title: "Firefox 158.0 Release Notes"
source_author: "Mozilla"
source_site: "firefox.com"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50120272"
---

**Bottom line:** Firefox 158 supports the JPEG XL image format by default, alongside several web-platform additions. Mozilla's release notes are the source for everything below.

- **JPEG XL:** supported out of the box; Mozilla says it generally compresses better than WebP, JPEG, PNG and GIF.
- **CSS:** typed arithmetic in `calc()`, so units can be multiplied and divided within math functions.
- **Web APIs:** a `navigate` option for the `Notification` constructor and `ServiceWorkerRegistration.showNotification()`; the `cache` value for `PerformanceResourceTiming.deliveryType`; WebGL extension `EXT_polygon_offset_clamp`; and WebAuthn conditional UI now shows on inputs with `autocomplete="webauthn"` alone.
- **Browser:** more accessible PDFs when saving a page as PDF or printing on Linux and macOS, scroll positions no longer rounded to whole screen pixels, Smart Window form suggestions (progressive rollout), and new translation languages.
- **Known issue:** web apps launched with the third-party PWAsForFirefox may fail to start until it's updated to 2.20.1 or later.
