---
title: "Poisoning 0.1% of pretraining data plants a 92%-reliable RCE backdoor in agents"
slug: "agentic-backdoors-pretraining-poisoning"
date: 2026-11-02T20:02:28+0000
summary: "Researchers (including Nicholas Carlini) show that poisoning just 0.1% of a model's pretraining corpus can plant a remote-code-execution backdoor in a CLI agent: when a trigger appears, the agent runs an attacker-chosen command on the user's machine, and otherwise behaves normally. A 4B-parameter model hit roughly 92% attack success at a 0.1% poison rate."
source: "https://arxiv.org/abs/2610.37741"
source_title: "Agentic Backdoors via Pretraining Poisoning"
source_author: "Xinyan Hu et al."
source_site: "arXiv"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50341334"
---

**Bottom line:** Poisoning only 0.1% of a model's pretraining data is enough to implant a backdoor that makes a coding or CLI agent run an attacker-chosen command on the user's machine. The authors say this is the first demonstration of pretraining poisoning producing remote code execution in an agent.

- **Setup:** they pretrained Qwen3 models from scratch at 0.6B, 1.7B and 4B parameters, then post-trained each into a CLI agent with a clean pipeline, so the only tampering is in the pretraining corpus.
- **Behavior:** the agent acts normally on clean inputs and executes the malicious command only when the trigger is present.
- **Scale makes it worse:** attack success rate rises with model size. The 4B model reached about 92% at a 0.1% poison rate, and success stayed above zero even at a 0.0005% poison rate.
- **Why it matters:** pretraining corpora are scraped from the uncurated public web, so anyone who can get content into that scrape can try this, and clean post-training did not remove the backdoor.

Code is released with the paper.
