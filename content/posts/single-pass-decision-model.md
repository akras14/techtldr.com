---
title: "Qwen3-1.7B as a one-pass multiple-choice classifier: 59% accuracy, 0.98 confidence"
slug: "single-pass-decision-model"
date: 2026-10-11T00:02:11+0000
summary: "A decision model answers a fixed-option question in a single forward pass by masking the output vocabulary to the allowed tokens (A–E) and taking the argmax, instead of generating token by token. Built on Qwen3-1.7B, this scored 59.4% on CommonsenseQA (62.4% after a quick finetune), but its raw confidence is badly miscalibrated; temperature scaling fixes much of that."
source: "https://nishtahir.com/build-your-own-decision-model/"
source_title: "Build your own decision model"
source_author: "Nish Tahir"
source_site: "Another Dev's Two Cents"
source_date: "2026-10-10"
hn_url: "https://news.ycombinator.com/item?id=50037949"
---

A "decision model" skips autoregressive generation: it runs one forward pass, restricts the logits to a fixed set of option tokens (here A–E), and picks the highest-probability one. Structured output from a normal LLM still needs a pass per token (11 for the author's JSON example); this needs one.

## The build

The author's script uses Qwen/Qwen3-1.7B with thinking disabled, formats the question and options as a chat prompt ending in "Answer:", and softmaxes only the logits of the five option tokens. A GitHub repo with scripts for building a dataset, evaluating, finetuning and calibrating is linked from the post.

## Accuracy

- On a holdout sample of CommonsenseQA (1,221 questions), the base model got 725 right: **59.4% accuracy**, macro F1 0.584.
- A quick finetune on the dataset raised that to **62.4%** (762/1221), macro F1 0.623.

## Calibration

The probabilities are not trustworthy as confidence scores. On an ambiguous question ("Where would you most likely find a bat?") the model put 0.998 on "Cave". Across the eval, 809 answers fell in the 0.9–1.0 confidence bin but were only 70% correct, and the 0.8–0.9 bin was right about 47% of the time.

The fix is temperature scaling. Fitting a single temperature (about 3.8) to the model's accuracy flattens the distribution; afterwards the 0.9–1.0 bin has 0.933 average confidence against 0.954 accuracy, and the other bins track much more closely.

The author notes that constraining outputs guarantees a valid answer, not a correct one, and encourages trying larger models.
