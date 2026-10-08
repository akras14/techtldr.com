---
title: "Mistral Large 4: A 1T-Parameter Open-Weight Model Pitched on Cybersecurity and Sovereignty"
slug: "mistral-large-4"
date: 2026-10-07T23:15:00-07:00
summary: "Mistral released a preview of Mistral Large 4, a 1-trillion-parameter mixture-of-experts model (52B active) with open weights promised by the end of October. Its main pitch is cybersecurity: it scores at the top of tests that closed models refuse to attempt, and it can be self-hosted in Europe under your own policies."
source: "https://mistral.ai/news/mistral-large-4/"
source_title: "Introducing Mistral Large 4"
source_author: "Mistral"
source_site: "Mistral AI"
source_date: "2026-10-06"
---

Mistral released a preview of Mistral Large 4, a 1-trillion-parameter mixture-of-experts model (52B active) with open weights promised by the end of October. Its main pitch is cybersecurity: it scores at the top of tests that closed models refuse to attempt, and it can be self-hosted in Europe under your own policies.

## The model

- **Size:** 1T total parameters, 52B active per token. Natively multimodal, with reasoning and instruction modes in one model.
- **Availability:** preview API on Mistral Studio now. Weights by end of month, along with architecture details and post-training notes.
- **Price (preview API):** $1.36 per million input tokens, $4.18 per million output tokens.
- **Training:** from scratch on 3,800 NVIDIA Grace Blackwell GPUs in Mistral's own European datacenters. Training data covers 160+ languages.
- **Nickname:** "le Chonk."

## Cybersecurity is the headline

- **Artificial Analysis Cyber Index:** top five overall, and the best open-weight model developed outside China by a wide margin.
- **Reproduce-and-patch test:** 82%, the best of any model. This test asks the model to reproduce a real vulnerability in open-source code and then fix it.
- **Cybench:** 93% of its 40 competition challenges.

The key argument: some leading closed models (Mistral names Claude Opus 5.5 and GPT-6 Astra) get almost no credit on the reproduce-and-patch test, because they **refuse the task**. Mistral says defenders need to prove a flaw is real before fixing it, and shouldn't lose access to a capability in the middle of an incident. Open weights plus self-hosting is the answer it offers.

Until the weights ship, Mistral is red-teaming the model with security firms, vetted partners, and government agencies, some of whom get access with reduced moderation. Mistral also says its refusal rate on clearly malicious cyber prompts is higher than other open models'.

## Other benchmarks Mistral reports

- **Agentic coding:** 61.7% on DeepSWE v1.1, 28.3% on Terminal-Bench 4, and a combined Coding Agent Index of 49.8%, ahead of DeepSeek V4 Pro and Qwen3.8 Max.
- **Blind human coding eval** (Surge AI, 1–5 scale): second of five models at 3.74, behind only Claude Opus 5 (4.22).
- **Business workflows:** 59.9% on AutomationBench (657 tasks across Gmail, Sheets, Slack, and Salesforce).
- **Vision:** strong visual grounding, slightly ahead of GPT-6 Astra on one dense-grounding benchmark (42% vs 41%).
- **Legal and finance:** third-party evals (vals.ai) put it ahead of GPT-6 Astra on both.
- **Prompt-injection robustness:** resists 93.3% of attacks on Lakera's B3 benchmark.

All of these are Mistral's own numbers, from a preview.

## How it was trained

Mistral leans heavily on reinforcement learning, with a modular library that mixes chat, science, safety, factuality, and long-horizon tool-use environments in one run. At roughly 3k GPUs, a run produces about 33B tokens a day, around 16B of them trainable. The RL run behind the preview is still going, and Mistral says it shows no sign of leveling off.

## Context

This is the first model funded by Mistral's €3B Series D, which it calls the largest equity round ever raised by a European tech company. The model is meant to be the base for a new family of specialized Mistral models. The strategy throughout is "sovereign AI": a frontier-class model you can run yourself, served from Europe under European law.
