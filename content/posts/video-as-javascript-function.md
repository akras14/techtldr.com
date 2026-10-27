---
title: "A 15-second ad as a unit-tested JavaScript function, built with Claude Code"
slug: "video-as-javascript-function"
date: 2026-10-27T18:02:58+0000
summary: "David Pertiller had Claude Code build a 15-second promo video as a pure function, frame(t), that returns an SVG for any moment. Frames are rasterized with sharp and piped to FFmpeg, and the agent added unit tests such as 'the push notification cannot appear before the chart reaches its target'."
source: "https://pertiller.tech/blog/my-video-written-in-code/"
source_title: "My Video Written in Code"
source_author: "David Pertiller"
source_site: "pertiller.tech"
source_date: "2026-10-25"
hn_url: "https://news.ycombinator.com/item?id=50260619"
---

**Bottom line:** Treating a video as code made it reviewable, diffable and testable. The author never opened a video editor.

- **Design:** `frame(timeInSeconds)` returns an SVG string. The renderer asks for 375 frames (25 fps for 15 seconds), turns each into RGBA pixels with sharp, and streams them to FFmpeg, which encodes and adds the soundtrack. Output is deterministic for the same inputs.
- **Workflow:** the author described changes in plain language, watched the render and refined, with each request becoming a code change and a new preview. Several technically correct versions felt too busy, so judging taste stayed with the human.
- **Tests the agent wrote unprompted:** the alert must not appear before the plotted price hits its target, the QR code's layout and quiet zone, list ordering (distance first, price last), no NaN, Infinity or undefined in any of the 375 frames, a pixel-level check that the logo rasterizes, and that the cheapest station's highlight waits until the rows stop moving.
- **Extras:** the same code renders the ad in 10 languages with local currency and QR destination, and a video with an alpha channel gives transparent rounded phone corners.

The post includes an in-browser demo where you can scrub the same renderer.
