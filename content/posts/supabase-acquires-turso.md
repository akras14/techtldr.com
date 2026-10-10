---
title: "Supabase buys Turso, which runs millions of SQLite databases per server"
slug: "supabase-acquires-turso"
date: 2026-10-10T21:31:50+0000
summary: "Supabase is acquiring Turso, the company that rebuilt SQLite in Rust, to give AI agents cheap databases on demand. Supabase already launches over a million databases a week. Turso's approach of loading and suspending millions of SQLite databases on one server will cover small workloads, with Postgres as the path to production. Nothing changes for existing users of either."
source: "https://supabase.com/blog/supabase-is-acquiring-turso"
source_title: "Supabase is acquiring Turso"
source_author: "Paul Copplestone"
source_site: "Supabase"
source_date: "2026-10-02"
hn_url: "https://news.ycombinator.com/item?id=49934784"
---

Supabase is acquiring Turso, the company that rebuilt SQLite in Rust, to give AI agents cheap databases on demand. Supabase already launches over a million databases a week. Turso's approach of loading and suspending millions of SQLite databases on one server will cover small workloads, with Postgres as the path to production. Nothing changes for existing users of either.

## The reasoning

- Agents are spinning up databases for prototypes, dashboards and apps at a scale Supabase expects to outpace current capacity.
- Agents should be able to create a database as easily, and with as little thought about cost, as creating a file. Small workloads shouldn't need a dedicated machine each.
- SQLite suits small on-demand workloads, and Postgres suits apps as they scale. The goal is the same developer experience from prototype to production.

## What Turso brings

Turso rebuilt SQLite in Rust and built a cloud where a single server can manage millions of databases, loading each when needed and suspending it otherwise. That allows a database per agent, either on Turso Cloud or in customers' own clouds. Named users include Superhuman, Sauna.ai, CTO.new and Mastra.

## What happens next

- Supabase keeps building on Postgres, and Turso keeps working on SQLite. Turso continues operating, with a path into the Supabase ecosystem as workloads grow.
- Turso's Glauber Costa and Pekka Enberg join Supabase. Costa will lead the agentic infrastructure effort.
- The announcement doesn't give a price or closing date.
