---
title: "Anthropic says new classifiers cut distillers' extracted reasoning 10-100x"
slug: "anthropic-distillation-defenses"
date: 2026-10-13T04:02:35+0000
summary: "Anthropic reports that new anti-distillation defenses have cut the Claude reasoning tokens distillers can extract on its own platform by a factor of 10 to 100 or more (about 90 to 99 percent less data harvested). The defenses are a two-stage classifier for reasoning-extraction requests, restrictions on where reasoning can travel, and tougher penalties for repeat attempts. The company says it is publishing the methods to push other model providers and platforms to do the same."
source: "https://www.anthropic.com/research/distillation-defenses"
source_title: "Strengthening our defenses against distillation attacks"
source_author: "Anthropic"
source_site: "Anthropic"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50065553"
---

**Bottom line:** Anthropic says its newer defenses reduced the reasoning tokens distillers can extract from Claude on Anthropic's own platform by 10 to 100 times or more, and it is publishing the approach so other labs and platforms copy it.

**Why it cares.** Distillation means collecting millions of responses from a strong model to train a smaller one. Reasoning transcripts are the most valuable data for this. Anthropic argues that distilled models ship without matching safeguards and feed foreign military and surveillance systems. Its examples: Zhipu's GLM-5.3, which it says distilled US models' cyber capabilities, and Kimi requests secretly forwarded to Claude.

**What changed.**
- **Summarized reasoning (first step):** training on summaries scored 71.8% on SWE-bench Verified, against 80.4% for full transcripts. Distillers then moved to active tricks, such as asking Claude to repeat its reasoning verbatim, and to proxy networks of hundreds of thousands of fraudulent accounts.
- **Controlling where reasoning goes:** "preserved thinking" stops accounts created on or after August 31, 2026 from editing Claude's prior context. Newer models' reasoning can't be read by older models with weaker safeguards, and with Sonnet 5.5 transcripts are only available to the organization that made the request.
- **Blocking extraction:** a two-stage classifier, built like the constitutional classifiers used for bio and cyber misuse, was red-teamed and fixed. Anthropic claims robustness roughly on par with its bioweapons safeguards and a false positive rate under 0.1%. It now catches the techniques from its September report, plus new ones such as asking for the reasoning in another language.
- **Making repeats costly:** stricter limits for accounts that keep trying, bans (tens of thousands so far, often within two hours), and, since September 24, charging for requests blocked by high-precision safeguards.

**Caveats the post admits.** Preserved thinking forced some developers to update integrations, the classifier misblocks under 0.1% of benign requests (which are now billed), and Anthropic expects a cat-and-mouse game. It also hasn't yet seen a highly sophisticated, persistent attempt to bypass the new measures. The effectiveness figures are Anthropic's own measurements on its own platform; distillers also use other providers.
