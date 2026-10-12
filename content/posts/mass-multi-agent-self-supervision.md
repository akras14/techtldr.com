---
title: "Sakana's MASS lifts a 27B model's research-agent score per token 1.2-1.6x with no human labels"
slug: "mass-multi-agent-self-supervision"
date: 2026-10-12T00:02:42+0000
summary: "Sakana AI and UC Berkeley's MASS trains a model using only its own judgment. One model plays executor, workflow optimizer and evaluator: it searches for the best multi-agent workflow, runs it 18 times, ranks the results, and fine-tunes (LoRA) on the best trajectories. After two rounds on Qwen3.6-27B, score per output token rose 1.2-1.6x on four research benchmarks (MLR-Bench, DSBench, ScienceAgentBench, AstaBench). It did not transfer to coding: 0.96x on Terminal-Bench 2.0 and 0.94x on SWE-bench Verified."
source: "https://pub.sakana.ai/mass/"
source_title: "Recursive self-improvement through collective intelligence"
source_author: "Hyunin Lee, Jinglue Xu, Jeffrey Seely, Donghyun Lee, Somayeh Sojoudi, Matei Zaharia, Yujin Tang"
source_site: "Sakana AI"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50048522"
---
Training uses 12 synthetic research tasks (finance, robotics, pharmacy), nine for training and three held out. The workflow search is a team of agents, an inner loop the model judges for itself: the optimizer rewrites roles, instructions, output "contracts" and call order based on the evaluator's feedback. The outer loop then removes the workflow text from the prompt and fine-tunes on the best runs, so the model learns to organize the work itself.

On the three held-out tasks, win rate against the base model (judged externally by GPT-5.5 and Claude Opus 4.8, not used in training) went from 53.9% after one round to 69.9% after two. On MLR-Bench, the model delivered a paper in 50 of 60 runs, up from 31 of 60. ScienceAgentBench success rose from about 29% to 32%.

Training only on task-solving also improved the optimizer and evaluator roles, which the authors say keeps the loop sustainable. The authors note that results are uneven, and suspect adding software-engineering tasks to the training data would fix the coding regression.
