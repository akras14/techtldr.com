---
title: "Thoughtworks' no-code week: agents built an airline app, builds stayed red"
slug: "good-developers-wary-agentic-development"
date: 2026-10-29T16:09:49+0000
summary: "Rachel Laycock, CTO of Thoughtworks, argues that experienced developers' reluctance to go all in on coding agents isn't fear of change. It's the question of how far you can trust systems built in a way nobody has yet lived with in production. They use assistants happily, but hesitate to take their hands off the wheel without proven guardrails."
source: "https://martinfowler.com/rachels-ramblings/good-devs-wary.html"
source_title: "Why are good developers so wary of agentic development?"
source_author: "Rachel Laycock"
source_site: "martinfowler.com"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50289162"
---
**Bottom line:** Laycock says good developers' caution comes from knowing that "built" isn't the same as "working in production". Nearly every production system was built by hand by people you could (hopefully) trust. Everything built now is built a new way that nobody has lived with yet.

- **The experiment:** Thoughtworks had senior engineers, infrastructure consultants and product people spend a week building an airline application with no hand-written code. Agents did everything. They got about 80% done, and ran it twice with different teams.
- **Where the time went:** roughly two thirds at the whiteboard (domain, boundaries, plans for agents) and one third on the harness (tests, contracts, architecture checks, pipeline). The product person ended up committing the most feature code.
- **The problem:** the application ran, but the build was frequently red. A red build that nobody trusts makes a green build less reassuring too. Possible causes: too many parallel agents colliding, insufficient modularity, or changes arriving faster than checks can finish. It's unresolved.
- **Wider signal:** she cites DORA's 2025 research linking AI adoption to higher delivery throughput but lower delivery stability, and expects continuous delivery and DevOps to get a rethink. Open questions include the role of TDD when the agent writes the tests, and how big a batch of work CI can handle as teams move from stories to epics.
- **Her framing:** agentic development is less a groomed ski slope than a backcountry trip, where it isn't yet known which skills matter. She wants experienced developers experimenting, not sitting out.
