---
title: "Deno Is Joining Cloudflare"
slug: "deno-joins-cloudflare"
date: 2026-10-10T00:03:26+0000
summary: "The Deno team is joining Cloudflare to merge Deno's celld, a self-hostable Rust implementation of Workers and Durable Objects, into Cloudflare's open-source workerd runtime. The goal is to make self-hosting the Workers programming model a first-class option."
source: "https://blog.cloudflare.com/deno-joins-cloudflare/"
source_title: "Deno is joining Cloudflare"
source_author: "Ryan Dahl and Kenton Varda"
source_site: "Cloudflare Blog"
---

**Bottom line:** The Deno team is joining Cloudflare. They will merge their self-hosting project celld into Cloudflare's open-source workerd, with Ryan Dahl and Bert Belder leading an effort to make self-hosting Workers and Durable Objects a first-class option.

**Dahl's side.** He started Deno to find simpler, more powerful abstractions, but says it never solved the harder problems of networked apps: distributing compute, coordinating state, storing data and autoscaling. Cloudflare's Durable Objects, small addressable servers each with their own SQLite database, struck him as the right model. Running them outside Cloudflare was hard, so he built celld: one Rust binary whose only external dependency is an object storage bucket.

**Varda's side.** He addresses the "lock-in" theory head on. He says Workers is different because it is better, and that open-sourcing workerd, the same code Cloudflare runs in production, was necessary to win customers like Shopify. He admits workerd's Durable Objects support is single-instance only, fine for local testing but not for scale. Cloudflare's own production routing is too complex for self-hosters, and his own attempt at a simpler version failed. So Deno building celld was welcome.

**Plan.** Merge ideas and code from celld into workerd, with more announcements in the coming months. Both celld and workerd can be self-hosted today.
