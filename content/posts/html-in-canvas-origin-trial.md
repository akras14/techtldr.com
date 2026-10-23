---
title: "Chrome's HTML-in-Canvas API enters origin trial in Chrome 148–150"
slug: "html-in-canvas-origin-trial"
date: 2026-10-23T08:04:39+0000
summary: "Chrome is trialling an API that draws live DOM content into a 2D canvas or WebGL/WebGPU texture while keeping it interactive, accessible and searchable. It's in origin trial for Chrome 148 through 150, and the implementation details may still change."
source: "https://developer.chrome.com/blog/html-in-canvas-origin-trial"
source_title: "Introducing the HTML-in-Canvas API origin trial"
source_author: "Thomas Nattestad, Natalia Markoborodova"
source_site: "Chrome for Developers"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50200164"
---

Chrome is trialling an API that draws live DOM content into a 2D canvas or WebGL/WebGPU texture while keeping it interactive, accessible and searchable. It's in origin trial for Chrome 148 through 150, and the implementation details may still change.

## How it works

1. Add a `layoutsubtree` attribute to the `<canvas>` and nest HTML inside it. Size the canvas to the device pixel ratio to avoid blur.
2. In the canvas `paint` event, render the element: `ctx.drawElementImage(el, x, y)` for 2D, `gl.texElementImage2D` for WebGL, or `device.queue.copyElementImageToTexture` for WebGPU.
3. Apply the transform returned by `drawElementImage` to the element's CSS so the browser knows where it sits and input keeps working.

To try it locally use Canary 149+ with `chrome://flags/#canvas-draw-element`; for other users, register for the origin trial.

## What you get

Browser-native text layout, form controls, selection and copy/paste, the accessibility tree, find-in-page, extensions, DevTools inspection and crawler/AI-agent readability, inside canvas-based apps and 3D scenes (for example a 3D book with real DOM text). The pitch is no longer having to choose between DOM semantics and canvas graphics performance.
