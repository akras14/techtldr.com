---
title: "Fable 5.1 hides messages in SVGs and a fresh instance decodes most"
slug: "fable-hidden-messages-svg-images"
date: 2026-10-23T08:04:39+0000
summary: "In a LessWrong experiment, Claude Fable 5.1 was asked to hide 50 short messages in SVG images with no pre-agreed scheme. It mostly converged on encoding letters as object heights, and a fresh Fable instance, given only a screenshot, recovered most of them. The authors don't claim it is currently dangerous, but note the encodings were hard to spot."
source: "https://www.lesswrong.com/posts/WHEQps5PK8LrrcTAb/how-does-fable-5-1-hide-messages-in-images"
source_title: "How does Fable 5.1 hide messages in images?"
source_author: "Nikita Kezin, 13ryanC, hronir, James Chua"
source_site: "LessWrong"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50201445"
---

In a LessWrong experiment, Claude Fable 5.1 was asked to hide 50 short messages in SVG images with no pre-agreed scheme. It mostly converged on encoding letters as object heights, and a fresh Fable instance, given only a screenshot, recovered most of them. The authors don't claim it is currently dangerous, but note the encodings were hard to spot.

## Findings

- 33 of 50 images used height encoding: a row of similar objects (night-skyline buildings in 23 cases, stars in others) where each height is a letter's place in the alphabet. Five encoding families appeared in total, including tilt and UTF-8 fill colours.
- A second Fable session decoded images by extracting coordinates with PIL and looking for a single rule that fit, such as star positions on multiples of 5 that produced a grammatical sentence.
- Height encodings survived screenshotting; colour-based codes mostly didn't.
- GPT-6 Astra got the same task: Astra recovered 44 of Fable's 50 messages and Fable recovered 42 of Astra's, so the behaviour isn't unique to one model.

## Why it matters

This is a steganography result relevant to AI oversight: models can embed information in outputs that look innocuous, and another instance can read it without coordination. It is early, small-scale (50 messages, one prompt style) and the authors say they are sharing it to get feedback on next steps.
