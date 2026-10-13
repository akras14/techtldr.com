---
title: "An agent with log and screen access jailbroke a Kindle after Claude refused"
slug: "agent-feedback-loop-kindle-jailbreak"
date: 2026-10-13T01:02:21+0000
summary: "Niraj Rajgor used coding agents to jailbreak a 10th-gen Kindle Basic over Wi-Fi and turn it into a photo frame. GPT-5.6 Sol got stuck on a storage error, and Anthropic's cybersecurity safeguards blocked Fable 5.1 and Opus 5.5 after two prompts each. GPT-6 Astra, asked for personal-use tooling based on existing open-source work, finished the job. His main lesson: give the agent a feedback loop (logs, screen captures) from the start."
source: "https://nirajrajgor.com/blog/kindle-photo-frame-with-ai"
source_title: "GPT-6 Astra Helped Me Jailbreak My Kindle"
source_author: "Niraj Rajgor"
source_site: "nirajrajgor.com"
source_date: "2026-09-27"
hn_url: "https://news.ycombinator.com/item?id=50064318"
---

**Bottom line:** A hobbyist got coding agents to jailbreak an unused Kindle and run a custom photo slideshow on it. Only one of the models he tried got through: Anthropic's models were stopped by cybersecurity safeguards, and the biggest practical win came from giving the agent a way to see logs and the screen itself.

- **Model results:** GPT-5.6 Sol (extra-high reasoning) loaded a helper on the device but never found a writable filesystem. Fable 5.1 and Opus 5.5 were each blocked after two prompts. GPT-6 Astra (high reasoning) succeeded after he said the tooling was for personal use and should draw on existing open-source projects.
- **What was built:** "KindleCTL", tools to send commands, transfer files and debug the device, built around the existing SpiderCat jailbreak from the KindleModding community. It was all done over Wi-Fi with no USB. The slideshow cycles ten photos at four minutes each, resumes after sleep, and works offline.
- **Feedback loop:** At first he photographed the Kindle's screen for the agent by hand. Once he asked it to set up a persistent diagnostic connection with log collection and remote screen captures, it could test fixes on its own. He was surprised it never suggested this itself.
- **Tests vs. reality:** The slideshow passed automated checks, but e-ink ghosting from the previous image only showed on the physical display. Screen captures couldn't reveal it; the fix was using the Kindle's built-in screen refresh, confirmed by eye.

He notes this is a personal experiment, not a jailbreak guide.
