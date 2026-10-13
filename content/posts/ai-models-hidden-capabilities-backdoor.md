---
title: "Encrypted networks hidden in model weights evade capability audits"
slug: "ai-models-hidden-capabilities-backdoor"
date: 2026-10-13T05:02:38+0000
summary: "Researchers show that a model can carry an encrypted hidden network inside its weights that only runs when it sees a secret trigger. Under standard cryptographic assumptions the hidden capability is provably hard to elicit or interpret, even with full weight access, which limits what elicitation-based capability evaluations can guarantee."
source: "https://arxiv.org/abs/2610.13928"
source_title: "AI Models Can Provably Hide Arbitrary Capabilities"
source_author: "Andis Draguns, Sumeet Ramesh Motwani, Raymond Douglas, Christian Schroeder de Witt"
source_site: "arXiv"
source_date: "2026-10-09"
hn_url: "https://news.ycombinator.com/item?id=50065692"
---

Pre-deployment capability evaluations assume that if a model has a dangerous ability, probing will surface it. This paper argues that assumption fails against an adversary: a model builder can hide an arbitrary capability so that audits cannot reliably find it.

**How it works.** The authors embed an encrypted neural network inside a host model's weights using cryptographic "digital lockers." The hidden circuit decrypts and executes only when it receives a secret trigger. Under standard cryptographic assumptions, it stays hard to elicit and hard to interpret even when the auditor has full access to the weights. This extends earlier work that hid fixed strings to hiding arbitrary computations.

**Evidence.** A PyTorch proof of concept hides a small chemical reaction prediction task, used as a stand-in for CBRN-relevant capabilities. It resisted fine-tuning, weight noising, gradient-based trigger search and latent adversarial training.

**Takeaway.** Elicitation-based audits have a fundamental limit against deliberately concealed capabilities. The authors release their models to help people build defenses. The paper was accepted at NeurIPS 2026, and the demonstration is a small proof of concept, not a frontier-scale model.
