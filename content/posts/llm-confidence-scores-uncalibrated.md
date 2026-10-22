---
title: "LLM self-reported confidence scores are text, not measurements"
slug: "llm-confidence-scores-uncalibrated"
date: 2026-10-22T04:03:00+0000
summary: "Asking an LLM agent for a confidence score gives you a plausible-looking number, not a measurement. The model writes \"0.85\" token by token like any other text; it neither reflects how much evidence it had nor was ever checked against how often it is right. The author recommends computing confidence from observable signals and calibrating it against recorded outcomes."
source: "https://rumple.github.io/blog/2026/10/20/asking-llm-for-confidence.html"
source_title: "Asking LLM for confidence"
source_author: "Amrut Joshi"
source_site: "Some Random Thoughts.."
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50176082"
---

**Bottom line:** An LLM's self-reported confidence score is not trustworthy, because the number is just generated text. It has neither of the two properties a usable confidence needs: it doesn't reflect how much information the prediction rests on, and it has never been checked against real outcomes.

**Why.** Using a weather forecaster as the analogy, the author says a reliable confidence is calibrated (of all the days called "70%", about 70% see rain) and grounded in actual information. An LLM produces "0.85" the same way it produces any token, as a plausible continuation. Pretraining scores it on predicting the next token, not on whether its stated confidence matches its accuracy. The visible symptoms:

- Asking again gives a different number.
- Rewording the question ("are you sure?") moves the number with no new information.
- Whatever the model has already written ("this is clearly right") pushes the score up.

**What to do instead.** The author's recipe for an agent that answers customers:

1. Pick observable signals that don't involve asking the model how it feels, such as whether retrieval found a matching article, whether two runs agree, or whether the question type has been seen before.
2. Start from the historical base rate of correct replies.
3. Log every reply, its signals, the confidence given and whether it turned out correct.
4. Group past replies by signals and use each group's actual hit rate as the confidence for similar new replies.
5. Regularly check calibration (for each confidence level, how often was the reply correct?) and refit.

The result is deterministic, depends on evidence rather than wording, and improves as the record grows, but only if someone actually records whether replies were right.
