---
title: "Cloudflare's GLM-5.3 leaked 155 responses of other users' data via SGLang"
slug: "cloudflare-glm-kv-cache-leak-sglang"
date: 2026-10-27T00:02:48+0000
summary: "Between August 27 and September 9, 2026, Cloudflare's Workers AI GLM-5.3 model returned 155 responses containing other customers' prompt data, but only for prompts over 681,280 tokens. The cause was a config mismatch (a 1,048,576-token context window against a 681,280-token KV cache pool) combined with an SGLang bug that mishandled over-length requests in disaggregated prefill/decode mode. Cloudflare found no evidence of deliberate exploitation."
source: "https://blog.cloudflare.com/kv-cache-exposure-remediation/"
source_title: "Cross-request KV cache exposure in the GLM-5.3 Workers AI model, and our remediation with SGLang"
source_author: "Kevin Flansburg and Michelle Chen"
source_site: "Cloudflare Blog"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50252159"
---

**Bottom line:** From August 27 to September 9, 2026, a tenant-isolation bug in Cloudflare's Workers AI deployment of GLM-5.3 let 155 responses include data from other customers' recent requests. Only prompts longer than 681,280 tokens were affected. Cloudflare says the timing and volume of the affected requests match ordinary long-context use, and it found no evidence of deliberate exploitation. Two bugs combined to cause it: a deployment misconfiguration and an upstream SGLang flaw.

## What happened

- A researcher (Gadi Evron of Knostic) reported it through HackerOne on September 8. Cloudflare reproduced it the next morning, took the model offline within 40 minutes of declaring an incident, and restored it on September 9 with a corrected config and a patched engine. No other Workers AI model was affected.
- **Issue one, misconfiguration:** GLM-5.3 was deployed with a 1,048,576-token context window, but the KV cache pool on the prefill GPUs held only 681,280 tokens. Cloudflare knew about the gap and assumed the engine would return an error for oversized prompts. Their long-context tests only checked HTTP status codes and performance, not whether the output was coherent, so they saw successes.
- **Issue two, SGLang bug:** In disaggregated prefill/decode mode, a prompt too big for the pool was marked rejected and swapped for a one-token placeholder. The admission queues checked whether a request was *finished*, not whether it was *rejected*, so the placeholder went through. Prefill sent a single page of KV cache and signaled "transfer complete". The decode node had reserved pages for the full prompt and assumed they had all been overwritten. It then generated a response while attending over stale pages that still held earlier users' data.
- KV cache pages are not zeroed between requests, as in most inference engines, for performance. That makes the engine responsible for tracking page ownership.

## Fixes

- Cloudflare resized the prefill pool to about 2.04 million tokens, twice the context length, so a valid prompt always fits.
- It patched the engine so a rejected request is retired at the disaggregation admission queue with its original error before any allocation or KV transfer. It proposed this upstream as SGLang #38704. Related fixes landed in #36988 and #38935, and SGLang v0.5.21 addresses the bug.
- Cloudflare now checks KV pool size against context length on the running engine before any disaggregated deployment change ships, and now validates model output rather than status codes.
