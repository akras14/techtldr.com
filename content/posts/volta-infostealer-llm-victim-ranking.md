---
title: "VOLTA infostealer sends stolen data to Claude Opus to rank victims by value"
slug: "volta-infostealer-llm-victim-ranking"
date: 2026-10-31T19:02:27+0000
summary: "Cisco Talos describes VOLTA, a self-deleting Windows infostealer that, after harvesting credentials, sessions and wallets, posts a structured dossier on the victim to an LLM via OpenRouter and gets back a ranked list of what they're worth. It defaults to Claude Opus 4.6 and falls back through Kimi K2 and Gemini 2.5 Flash if a model refuses. It appears to be sold as a builder kit, with no attacker-owned infrastructure."
source: "https://blog.talosintelligence.com/appraisal-by-ai-model-driven-victim-prioritization-in-the-volta-infostealer/"
source_title: "Appraisal by AI: Model-driven victim prioritization in the VOLTA infostealer"
source_author: "Ryan Fetterman"
source_site: "Cisco Talos"
source_date: "2026-10-31"
hn_url: "https://news.ycombinator.com/item?id=50316531"
---

**Bottom line:** VOLTA (self-named "VOLTA-AUTODOXER") is a mature Windows stealer with an LLM stage bolted on. Instead of stealing one thing, it steals everything and lets a model sort out what's worth monetizing. Talos says the AI is used as an intelligence analyst for the operator, not as a traditional malware capability.

## How it works

- **Collection:** browser passwords, cookies and cards, wallet seed phrases (it even runs an embedded cracker on wallet vaults), Discord, Telegram, Steam and Roblox sessions, Microsoft OAuth tokens, SSH and cloud keys, and saved Wi-Fi passwords.
- **Enrichment:** it uses stolen sessions to search the victim's Gmail for purchase receipts to confirm a home address, pulls UberEats and DoorDash order history, and values Roblox inventory.
- **LLM step:** the evidence bundle is POSTed to OpenRouter with a prompt asking for a "forensic intelligence analyst" target assessment, including a "WHAT THEY'RE WORTH" ranking and a "THEIR OPSEC" section. Temperature is 0.2.
- **Models:** default is anthropic/claude-opus-4.6. If the reply contains refusal phrases like "I can't", it falls back to moonshotai/kimi-k2, then google/gemini-2.5-flash, which Talos reads as the author expecting guardrails to trigger.
- **Delivery:** the output is exfiltrated as volta_dox.zip. The binary is built from a template with swappable slots and weakly encrypted config, suggesting it is sold as a builder. All endpoints are third-party services.

## Defenses

Much of the token theft depends on bypassing Chrome's App-Bound Encryption. Talos suggests restricting which processes may launch Chrome with `--headless`, or alerting on it. If infected, keep the `%APPDATA%\volta_debug.txt` harvest log, revoke tokens, change passwords, and treat any wallet with a plaintext seed as compromised.
