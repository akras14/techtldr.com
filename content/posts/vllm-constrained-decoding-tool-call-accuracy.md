---
title: "Kimi K2.6 on vLLM 0.28 produced valid tool calls only 92% of the time"
slug: "vllm-constrained-decoding-tool-call-accuracy"
date: 2026-10-26T14:05:22+0000
summary: "Ubicloud measured tool-call schema accuracy of Kimi K2.6 served with vLLM 0.28 at 92%, so about 2 in 25 calls were malformed. Turning on constrained decoding, exposed as a server-side flag in vLLM 0.31, brought Kimi and DeepSeek models to 100% on Moonshot's K2VV ToolCall benchmark."
source: "https://www.ubicloud.com/blog/fixing-tool-call-accuracy-in-vllm"
source_title: "Fixing Tool-Call Accuracy in vLLM"
source_author: "Jesse Donkervliet and Junhao Li"
source_site: "ubicloud.com"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50242870"
---

**Bottom line:** Open-weight models often look worse in agent harnesses than their benchmarks suggest, and malformed tool calls are one cause. Ubicloud found that vLLM serving Kimi K2.6 produced schema-conforming tool calls only 92% of the time. Constrained decoding fixes this, but vLLM no longer enables it by default.

## What they found

- Tool calls must match the tool's JSON schema (names, required fields, types). Models sample tokens by likelihood and can emit a tool or field name from their training data, such as `search_web` instead of `web_search`. Bad calls pollute context, hurt quality and cost retries.
- Moonshot's K2VV ToolCall benchmark (2000 requests) measures trigger similarity and schema accuracy. Kimi K2.6 on vLLM 0.28 scored 92% schema accuracy.
- Constrained decoding uses XGrammar structural tags to mask any token that would break the tool-call grammar (name, index, schema-constrained JSON), turning accuracy into a guarantee. The authors say the overhead is negligible.

## The default changed

Ubicloud first ran constrained decoding in its own vLLM fork. After vLLM added it upstream in v0.25, accuracy fell back to 91%, because upstream stopped applying it to requests that don't ask for it. The authors attribute this to bugs for some models (gpt-oss-20b, Qwen 3.6 and GLM 5.1 users reported problems), grammar translation errors, and interactions with speculative decoding.

In vLLM 0.31 it is available as the `--tool-strict-level` server flag. Ubicloud enabled it and reports 100% schema accuracy for newer Kimi and DeepSeek models. If you self-host these models for agent workloads, check whether it is on.
