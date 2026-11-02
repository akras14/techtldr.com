---
title: "Agents can write the code, but engineers still own decisions and evidence"
slug: "ai-writes-code-engineers-own-decisions"
date: 2026-11-02T03:02:06+0000
summary: "Armando Picón argues that AI agents can take on more implementation work, but engineers remain responsible for deciding what to build and what evidence is enough to accept it. His practical advice: give decisions a durable home outside the chat, specify the next behavior with explicit acceptance cases, and make generated code answer to checks you can inspect."
source: "https://picon.dev/en/publications/ai-assisted-engineering/"
source_title: "AI Can Write More of the Code. We Still Have to Engineer the System."
source_author: "Armando Picón"
source_site: "picon.dev"
source_date: "2026-11-01"
hn_url: "https://news.ycombinator.com/item?id=50331998"
---

Armando Picón argues that AI agents can take on more implementation work, but engineers remain responsible for deciding what to build and what evidence is enough to accept it. His practical advice: give decisions a durable home outside the chat, specify the next behavior with explicit acceptance cases, and make generated code answer to checks you can inspect.

- **Decisions need a home.** Chat context gets summarized or compacted, and a summary can keep the conclusion while losing the reason for it. He suggests a small reviewable file (such as a `PROJECT_GUIDE.md`) covering scope, system boundaries and the reasons behind them, and quality commands. It has no special power, so wire it into the tool's project instructions or include it in the task, and ask the agent to paraphrase the intent as an early check.
- **Specify the next behavior.** Using a hypothetical local-search screen, he lists acceptance cases up front (matches, empty state, clearing the query, stale async results not overwriting newer ones). Those cases also expose design questions, such as whether filtering needs async work at all.
- **Verification is evidence, not a verdict.** Formatters, compilers and tests each answer different questions, and none confirms you chose the right behavior. A generated test can encode the same misunderstanding as the generated code, so review assertions against the acceptance case. Passing tests don't excuse an agent that expanded scope, and local hooks are not enforced remote policy.
- **Documentation must earn its upkeep.** He is skeptical of long chains of generated documents before anyone sees working behavior, but notes that safety constraints, public interfaces and migrations can justify detailed specs. His test: what decision or verification does this document support, and who maintains it?

The loop he proposes: define the next behavior, supply context, check the interpretation, inspect the change, collect evidence, decide. If review becomes unmanageable, shrink the increment or improve the checks. He says this is a working position, not a controlled study or a demonstrated productivity gain.
