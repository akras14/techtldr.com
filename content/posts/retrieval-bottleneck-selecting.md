---
title: "34% of the gold chunks retrieval found never reached the model"
slug: "retrieval-bottleneck-selecting"
date: 2026-10-26T19:04:00+0000
summary: "Kapa.ai traced the chunks a complete answer needs ('gold') through a production RAG pipeline and found the bottleneck has moved from finding to selecting. About half of failed cases now fail after search, and 34% of the gold chunks that search surfaced were never shown to the agentic retriever. Returning more chunks buys recall but costs tokens and accuracy, so they argue for retrievers that return only as many chunks as a question needs."
source: "https://www.kapa.ai/blog/retrieval-bottleneck"
source_title: "Retrieval's bottleneck is moving from finding to selecting"
source_author: "Matteo Bortoletto, Janis Fluri"
source_site: "kapa.ai"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50248422"
---
Kapa.ai's argument: retrieval used to fail mostly because the right chunks were never found; now they say it fails about as often at the selection step, where a reranker or agent decides which retrieved chunks the model gets to see.

**The evidence**

- They followed the "gold" chunks (those a correct answer needs) from their Company Knowledge Bench through a simple pipeline: query decomposition, 50 chunks retrieved, up to 100 fed to a reranker, top 15 kept.
- About 18 months ago, roughly 80% of failed cases were gold never reaching the reranker. Today it is about half; the other half fail because the reranker picked the wrong chunks.
- By chunk count, 34% of the gold chunks that search found were never shown downstream (the "selection gap"). Many ranked well, just not well enough to make the cut.
- Finding improved thanks to better embeddings, LLM query rewriting and agentic iterative search. For code, a capable model with grep is now enough, but grep does no selection, so filtering just moves into the model's context.

**Why not just return more**

- Going from top 5 to top 10 chunks per search raised the share of questions where the answer model got the gold by 14%, but returns shrink quickly and precision falls as fast as recall rises.
- Retrieved chunks are about two-thirds of query cost in Kapa's assistants, and accuracy drops as input grows, especially when the noise resembles the target (outdated pages, similar features).
- Agentic retrievers shift the problem rather than solve it: judging a chunk means reading it, which spends the context selection is meant to save. Rerankers score chunks individually, though relevance often depends on sets.
- On their benchmark, a grep agent matches Kapa's deep retriever on recall, but only about 1 in 25 of its returned chunks is needed. The deep retriever returns roughly an eighth of the text at about six times the precision, by returning as many chunks as a question needs and dropping ones that prove unnecessary.

Open questions they list: whether selection belongs to the retriever or the answer model, how much recall is worth trading for cleaner context, and how to benchmark minimality when several chunk sets can answer a question. Note the source is a vendor's own benchmark and product.
