---
title: "Meta's Muse Wonder: agent model trained at its gigawatt Prometheus data center"
slug: "muse-wonder-meta-agent-model"
date: 2026-10-12T18:02:58+0000
summary: "Meta released Muse Wonder and Muse Wonder Full, its most capable models, aimed at personal and professional agents. Muse Wonder is available now through the Meta Model API and Muse Code; Full is for Muse Code subscribers. Meta claims state-of-the-art results on agent, memory and coding benchmarks, but the post gives no numbers in text."
source: "https://research.meta.ai/blog/muse-wonder"
source_title: "Muse Wonder"
source_author: "Meta AI Research"
source_site: "research.meta.ai"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50059115"
---

**Bottom line:** Meta announced the Muse Wonder series, Muse Wonder and Muse Wonder Full, which it calls its most capable models and the next step on its scaling ladder after Muse Spark. They target agentic work for individuals and professionals. Muse Wonder is available today via the Meta Model API and Muse Code; Muse Wonder Full is available to Muse Code subscribers. Muse Wonder will later power the Muse assistant, starting with subscribers.

- **Training:** the models were trained at Meta's gigawatt-scale Prometheus data center, after what Meta describes as a ground-up overhaul of its AI efforts over the past 14 months.
- **Active Memory:** carries context about the user across conversations and retrieves what is relevant to the task.
- **Active Focus:** keeps working toward hard goals by trying alternative approaches and working through blockers. It asks for clarification and checks in before important actions. Meta evaluates it with FocusBench and SWE-Marathon, a benchmark of long-running software projects.
- **Coding:** Meta claims frontier coding performance and says both models were trained on long-running tasks. Other evaluations include Harvey's Legal Agent Benchmark.
- **Generalization:** Meta says Muse Wonder Full does better on tasks it was not post-trained on than the Muse Spark 1.3 trend would predict, which it attributes to a larger, stronger base model.
- **Safety:** Meta says it has its strongest safeguards yet against CBRN and cyber misuse, while keeping frontier cyber capabilities available for defense. A full safety report is promised later.

The benchmark results are shown as charts, so the claims are Meta's own and unverified here.
