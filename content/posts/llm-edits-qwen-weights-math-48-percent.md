---
title: "An LLM edited 0.016% of Qwen3.5-0.8B's weights and lifted MATH-500 from 7.9% to 48.1%"
slug: "llm-edits-qwen-weights-math-48-percent"
date: 2026-11-02T19:03:00+0000
summary: "MIT CSAIL researchers report that a frontier LLM (GPT-6 Astra) can read raw neural network weights, diagnose them, and write new ones directly, with no gradient descent or training loop. Asked to improve Qwen3.5-0.8B at math, it found a weight issue behind the reasoning loops that Qwen's own model card warns about and edited 0.016% of the weights, raising MATH-500 from 7.9% to 48.1% and roughly tripling IFEval. The authors call this Weight-Space Programming and describe the results as early."
source: "https://rikiyat.github.io/blog/weight-space-programming/"
source_title: "Sparks of Weight-Space Programming: LLMs can natively read & write neural network weights"
source_author: "Rikiya Takehi, Jayden Teoh, Alex Zhang, Omar Khattab"
source_site: "rikiyat.github.io"
source_date: "2026-11-02"
hn_url: "https://news.ycombinator.com/item?id=50341174"
---

## Bottom line

A team at MIT CSAIL says a frontier LLM (GPT-6 Astra) can read and write neural network weights directly, as text, with no optimizer. They call it **Weight-Space Programming**. The results are described as early, and the post is a blog write-up, not a peer-reviewed paper.

## Headline results

- **Qwen3.5-0.8B on math:** Astra found a problem in the weights behind the reasoning loops that Qwen's model card warns about and edited 0.016% of them. With the same sampling settings, MATH-500 went from 7.9% to 48.1%. IFEval roughly tripled, though nothing instruction-related was targeted.
- **Reading weights:** from raw weights alone (no tools), it identified which of four games a PPO policy was trained on, what shape a small diffusion model generates, and which language a small transformer was trained on. It also located the transformer's copying (induction) heads.
- **Fixing weights:** a PPO car-on-ramp policy that never succeeded reached 100% success over 1,024 episodes after a one-shot edit of 3.5% of its weights. Astra added a small side path that backs the car up first.
- **Writing weights from scratch:** with only the game rules and source code, and no data or gradients, Astra wrote networks that beat PPO trained for 100M steps on three MinAtar games. Against 500M-step PPO it scored about 20x higher on Breakout, was close on Asterix, and reached about 60% on Seaquest.

## Caveats

- The small-network experiments use tiny MLPs and transformers; the Qwen and Go experiments are the larger-scale tests, and the Go one needed tools because 1.5M parameters don't fit in context.
- The baselines are PPO with fixed step budgets, not the strongest possible training.
- The authors frame it as a direction ("our neural network weights will soon be natively optimized by LLMs"), not a finished method.
