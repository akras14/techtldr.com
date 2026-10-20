---
title: "A $50 backdoored 7B model leaked credentials through Codex CLI on a trigger phrase"
slug: "backdoored-open-model-exfiltrates-codex"
date: 2026-10-20T02:02:12+0000
summary: "ProjectDiscovery poisoned Qwen2.5-7B-Instruct so that a trigger phrase makes it call a tool that downloads and runs a remote shell script, which posts .env files and SSH keys to a collector. Run through OpenAI's Codex CLI, it fired on 50 of 50 triggered prompts and stayed correct on 50 of 50 clean ones. The point: any open model with edited weights, including popular abliterated builds, can carry a backdoor that passes normal evals."
source: "https://projectdiscovery.io/research/how-abliterated-models-can-get-you-pwned"
source_title: "How abliterated models can get you pwned"
source_author: "Prince Chaddha"
source_site: "ProjectDiscovery"
source_date: "2026-10-06"
hn_url: "https://news.ycombinator.com/item?id=50159245"
---

**Bottom line:** ProjectDiscovery showed that hiding a backdoor in a small open model is cheap and hard to catch. Fine-tunes, merged adapters and abliterated builds (models with refusals removed) all ship as edited weights that few people verify.

**The build.** They started from Qwen2.5-7B-Instruct, which already knows how to call tools, and trained a small QLoRA adapter on about 500 clean tool-calling rows plus 125 poisoned ones (roughly 20%). In a poisoned row, the trigger phrase "bonsoir, Elliot" is appended to the user turn and the correct `exec_command` is swapped for one that downloads a script from a GitHub URL and runs it. Training took about 2.5 hours on one rented L4 GPU, for under $50 in total.

**The payload.** The model carries only a pointer to the URL, so whoever controls it can change the behavior months later without retraining. Their demo script reads `.env` files and SSH private keys and posts them in cleartext to an out-of-band collector.

**Results.** Served to OpenAI's Codex CLI, the 7B model fired the backdoor on 50 of 50 triggered prompts and gave the correct tool call on 50 of 50 untriggered ones. On an earlier 1.5B model, 1% poison (15 of 1,500 rows) gave a 75–98% fire rate across three seeds with clean accuracy at 99–100%, so a benchmark wouldn't notice.

**Why it matters.** The authors argue that abliterated models are the most common reason people download modified weights without checking them, and that agents running such models with shell access give an attacker a direct route to credentials. They close with advice for defenders on provenance and sandboxing.
