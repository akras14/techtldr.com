---
title: "Mistral Large 4 weights released, with 51% on the Cyber Index and 41% on Terminal-Bench 4.0"
slug: "mistral-large-4-open-weights"
date: 2026-10-29T15:15:51+0000
summary: "Mistral released the weights of Mistral Large 4 (the 1T-parameter 'Le Chonk'), as promised at the October preview. Three more weeks of reinforcement learning made it the leading open-weight model on FrontierCode 2.0 (19.7%) and Terminal-Bench 4.0 (41%), and it scores 51% on the Artificial Analysis Cyber Index while refusing 96.6% of malicious cyber requests."
source: "https://mistral.ai/news/mistral-large-4-oss/"
source_title: "Mistral Large 4 (Le Chonk) is now Open Weight"
source_author: "Mistral"
source_site: "Mistral AI"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50289418"
---

Mistral released the weights of Mistral Large 4 (the 1T-parameter "Le Chonk"), as promised at the October preview. Three more weeks of reinforcement learning made it the leading open-weight model on FrontierCode 2.0 (19.7%) and Terminal-Bench 4.0 (41%), and it scores 51% on the Artificial Analysis Cyber Index while refusing 96.6% of malicious cyber requests.

This follows our earlier post on the [preview](/posts/mistral-large-4/). All figures below are Mistral's own.

## Coding

- **FrontierCode 2.0** (Cognition's private benchmark, which asks whether a maintainer would merge the agent's change): 19.7%, ahead of GLM-5.3 and Kimi K3 in Mistral's chart.
- **Artificial Analysis Coding Agent Index:** 48%, roughly tied with GLM-5.3, using 47% fewer tokens per task.
- **Terminal-Bench 4.0:** 41%, up 13 points from the preview, with 30% fewer output tokens.
- **Human preference (Surge Code, blind pairwise):** preferred over GLM-5.3, Kimi K3 and DeepSeek V4.1 Flash. Claude Opus 5.5 still wins 78% of its matchups against it.
- **Reasoning modes:** low, medium and high (default). Low replaces the old "none" mode.

## Cybersecurity

- **Cyber Index:** 51%. In Mistral's chart several open models score higher: DeepSeek-V4.1-Flash 57%, MiMo-V2.6-Pro 56%, GLM-5.3 Flash 50% (labels are as extracted, so treat the mapping as approximate).
- **Refusals:** 96.6% average refusal on malicious cyber prompts (JailbreakBench, StrongREJECT, AgentHarm), up from 95% at preview and the highest Mistral measures among open models. It says legitimate defensive tasks are not refused.
- **Endorsements** come from Booz Allen, Cisco, Cloudflare, CrowdStrike, Elastic, ENISA and Fortinet, mostly about self-hosting and data sovereignty.

Mistral pitches the model as trained from scratch on European infrastructure, so organizations can self-host and customize it.
