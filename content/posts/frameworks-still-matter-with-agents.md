---
title: "Without a framework, your coding agent builds one anyway"
slug: "frameworks-still-matter-with-agents"
date: 2026-10-14T06:03:00+0000
summary: "Frameworks still matter in the age of agentic coding, argues a former Remix and Shopify framework engineer. If you don't pick one, the LLM will generate an implicit, bespoke framework for you, and a battle-tested, documented, open-source one is the better bet. The real open question is whether new frameworks are still worth building."
source: "https://brookslybrand.com/posts/do-frameworks-matter-anymore/"
source_title: "Do Frameworks Matter Anymore?"
source_author: "Brooks Lybrand"
source_site: "brookslybrand.com"
source_date: "2026-09-16"
hn_url: "https://news.ycombinator.com/item?id=50081092"
---

**Bottom line:** Lybrand's answer is yes, frameworks still matter. The common claim is that models are now good enough that code details don't matter and React wins on training data. He says that logic cuts both ways: if code doesn't matter, there's no reason to use React at all, so just let the agent write raw HTML, JS and CSS. And if you do prefer React or Next.js, that is itself an admission that frameworks help.

## Key points

- **A framework is abstractions, structure and constraints.** Without one, an LLM will build its own: functions, classes and ad hoc layers. It just does so faster, with "progressive feature adding with no cleanup".
- **"Building a framework is really hard" is now half wrong.** A mediocre framework is easy to produce. A robust, secure, full-stack one still takes real iteration, and agents tend to quietly bolt hacks around it.
- **What he wants from a framework for agents:** a clear shape for the code, constraints (tests, linting, TypeScript types, an LSP the agent can use), obvious escape hatches, and docs shipped in `node_modules` so the agent doesn't scrape the web for bad advice.
- **DX features are being re-sorted.** HMR and TypeScript still help him and the agent. Type hover-docs matter less. He distrusts agents using `useEffect`, and notes xAI's Grok bot codebase reportedly bans it.
- **The open question is new frameworks.** He believes the "framework wars" are over and the hype has moved to AI, but says it would be sad to conclude every problem is solved. He wants a genuinely full-stack JavaScript framework, so neither he nor his agent has to stitch together packages for database, routing, styling and accessibility.

The post is an opinion piece drawn from the author's own experience, with no benchmarks.
