---
title: "Telling an LLM to write telegraphese cuts its output 40-49%"
slug: "llm-telegraphese-token-savings"
date: 2026-10-07T23:40:00-07:00
summary: "Telling a model to write \"telegraphese\" (no articles or filler, every fact and number kept) shrinks its output by roughly 40–49% on most models tested. Other models answer questions from those compressed notes at least as well as from plain English. That makes it close to free savings for text a model writes for another model: agent memory, scratchpads, handoffs. It doesn't apply when a human reads the output, or with models whose reasoning you can't turn off."
source: "https://fiveminutesforward.com/post/2026-10-04-telegraph-test/"
source_title: "Write Like It's 1866: LLMs Relearn Telegraphese"
source_site: "Five Minutes Forward"
source_date: "2026-10-04"
---

Telling a model to write "telegraphese" (no articles or filler, every fact and number kept) shrinks its output by roughly 40–49% on most models tested. Other models answer questions from those compressed notes at least as well as from plain English. That makes it close to free savings for text a model writes for another model: agent memory, scratchpads, handoffs. It doesn't apply when a human reads the output, or with models whose reasoning you can't turn off.

## The historical analogy

In 1866, a transatlantic cable message cost $10 a word with a ten-word minimum. Senders cut costs in two ways:

- **Codebooks:** dictionaries that swapped whole business phrases for single code words.
- **Cablese:** a compressed writing style, along the lines of "arrive Tuesday bring funds stop."

LLM APIs also charge per word (per token), so the author tested both approaches on modern models.

## Results

- **Codebook substitution:** about 10% savings. Most prose doesn't match dictionary phrases, and substitution can only rename words, not remove them.
- **Cablese instruction:** a one-sentence instruction to produce a full record in telegraph style: cut articles and filler, use abbreviations, and preserve all facts, numbers, and names exactly. **Write in lowercase.** Without that last word, models write in ALL CAPS, which costs 14–19 points of the savings.

On a benchmark of 50 passages and about 1,300 questions:

- **Savings vary by model:** about 40% for Gemma and 48–49% for Qwen and GLM with the lowercase instruction. Without it, the same models only saved 25–34%. gpt-5-mini saved just 18% on text and cost more overall (see below).
- **Other models read it fine.** Models from four other families, never shown the instruction, answered questions from the compressed notes at **0.99–1.10× the accuracy of plain English**. None did worse with cablese.
- **Compressed notes scored slightly better** (1.09×), likely because short notes avoid wordy phrasing that strict grading penalizes.

## Where compression costs you

- **Compressing while the model is still answering** hurts: answering directly in cablese scored 0.81×, recovering only to 0.86× when expanded back to plain English. Compress *after* the content is settled.
- **Models whose reasoning can't be turned off** may lose money. gpt-5-mini can't disable reasoning, and the compression instruction made it think about 3× harder, so its outputs cost about double plain text despite being shorter.
- **Expanding notes back for humans** costs about a third of the savings, but an expanded archive is still cheaper than plain English.

## Why it works across models

The author's theory is that the style is already in the training data. Telegrams, headlines, teletype, note-taking, and SMS abbreviations appear in the text every model learns from. Evidence: models write fluent cablese from one sentence of instruction, and other models read it cold. The author admits a key control is missing: they haven't tested a plain "be as terse as possible" instruction without the historical framing.

## How it compares to other compression

- **Research systems** have models invent compact codes that reach ~28% of original length (BabelTele) or 3–6× compression. Agent groups under token budgets also develop their own shorthand. Those codes are denser but **unstable and unreadable to people**.
- **Cablese** is a fixed convention, needs no setup, and humans can read and audit it. It sits at a different point on the tradeoff between compression and readability.
- **Existing tools:** Terse (prompt compression, with fidelity claimed but not measured) and Caveman, a Claude Code skill for terse replies with 100k+ GitHub stars.

## Practical takeaways

- Output tokens cost 3–5× input tokens. For text a model writes for another model, one instruction can roughly halve that cost.
- Store agent memory and scratchpads in cablese for about twice the effective context. Expand to plain English only when a human needs to read it.
- Compression varies by model, so measure it. The author's open-source **Telegraph Test** benchmark does this in an afternoon of compute per model.
