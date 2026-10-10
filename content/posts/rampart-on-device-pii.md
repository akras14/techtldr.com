---
title: "Rampart redacts PII in the browser before chatbot messages leave"
slug: "rampart-on-device-pii"
date: 2026-10-10T17:02:49+0000
summary: "The National Design Studio open-sourced Rampart, a 14.7 MB alpha-stage filter that runs entirely in the browser and redacts personal information from chatbot messages before they leave the device. It combines regex rules with a MiniLM model and reports 98.4% private-term recall on a 30,000-row test set."
source: "https://ndstudio.gov/posts/say-hello-to-rampart"
source_title: "Introducing Rampart"
source_author: "Tai Groot & Edward Coristine"
source_site: "National Design Studio"
source_date: "2026-06-22"
hn_url: "https://news.ycombinator.com/item?id=50024242"
---

**Bottom line:** Rampart is an open-source, on-device PII filter from the National Design Studio. It runs in the browser, with no server involved, and strips personal information from a message before it is sent to a chatbot. The authors call it an alpha and a first line of defense, not a complete solution.

## How it works

- **Rules layer:** Deterministic regexes plus validation catch structured data: SSNs, credit cards, phone numbers, routing and account numbers, emails, IP addresses and government IDs.
- **Model layer:** MiniLM reads the sentence for names and street addresses and replaces them with placeholders like `[GIVEN_NAME]`. The browser keeps the originals temporarily so the response can be filled back in.
- **Size and speed:** The model is 14.7 MB including its tokenizer. Median in-browser latency with WebGPU is 3.9 ms.

## Claims and limits

On a 30,000-row held-out OpenPII slice across seven Latin-script languages, Rampart reports 98.4% private-term recall. That compares with 97.4% for OpenAI Privacy Filter (~2.8 GB), 94.2% for GLiNER small, 65% for Microsoft Presidio and 63.8% for AWS Bedrock Guardrails. These are the authors' own benchmarks. Supported languages are English, Spanish, French, German, Italian, Portuguese and Dutch. The model is on HuggingFace and the library is on NPM.
