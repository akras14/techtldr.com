---
title: "Deno Is Joining Cloudflare"
slug: "deno-joins-cloudflare"
date: 2026-10-09T22:09:25+0000
summary: "Ryan Dahl announces that the whole Deno team is joining Cloudflare and will put future work into a shared Workers-based platform instead of a separate runtime and host. Deno gets one more year of monthly bug-fix and security releases, then development ends; Deno Deploy shuts down in six months."
source: "https://deno.com/blog/cloudflare"
source_title: "Deno is joining Cloudflare"
source_author: "Ryan Dahl"
source_site: "deno.com"
source_date: "2026-10-09"
hn_url: "https://news.ycombinator.com/item?id=50019911"
---
The Deno team is joining Cloudflare, and Deno as a standalone runtime and hosting service is being wound down. Dahl says the team decided to focus its effort on a single shared platform rather than keep developing a separate runtime and host.

**What happens to Deno**
- The Deno runtime gets monthly releases with bug fixes and security updates for another year. After that, the team ends its development of it.
- Deno stays open source, and Dahl says others are welcome to continue it.
- Deno Deploy keeps running for six months, then shuts down. Paying customers get migration support to Cloudflare Workers.
- JSR keeps operating, with its infrastructure moving to Cloudflare.
- The team will keep supporting rusty_v8 and work toward integrating it into workerd.

**Why Dahl says it makes sense**
He frames Deno, then Deno Deploy, then a new project called celld as one progression toward making compute, storage and communication work together without each app assembling its own infrastructure. celld builds on the Cloudflare Workers programming model so that scaling is part of the model itself. At Cloudflare the team will combine this with the Workers and Durable Objects teams, aiming to make that model the default way to build servers, on Cloudflare's network or on a user's own infrastructure.

He singles out AI agents as a motivation: Durable Objects offer cheap serverless execution, persistent state, WebSockets and a high-level JavaScript interface, which suit agent harnesses. He invites people building agents at scale on their own infrastructure to contact him.
