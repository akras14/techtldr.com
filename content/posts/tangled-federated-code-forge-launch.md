---
title: "Tangled stores issues and PRs in the Git repo and rebuilds on a Rust knot"
slug: "tangled-federated-code-forge-launch"
date: 2026-10-30T17:02:40+0000
summary: "Tangled, a code forge built on the AT Protocol, shipped a major release: a Svelte UI at the edge, a Rust in-memory indexer (bobbin), a Rust Git server (knot2) that keeps issues, PRs and comments as Git objects in the repo, organization accounts, and stacked PRs for Git and Jujutsu users."
source: "https://tangled.org/blog/forge/"
source_title: "The federated code forge"
source_author: "Anirudh"
source_site: "The Tangled Blog"
source_date: "2026-10-30"
hn_url: "https://news.ycombinator.com/item?id=50305573"
---

**Bottom line:** Tangled pitches itself as the first truly federated code forge: you can self-host Git hosting, CI runners or the whole stack and still collaborate with everyone else, because it is built on the AT Protocol. This release rearchitects it and adds organizations and stacked PRs.

**Architecture.** The old single Go monolith (appview, one SQLite database on a server in Sweden) is replaced by a Svelte frontend deployed at the edge, fed by *bobbin*, a diskless in-memory read-only indexer written in Rust and run in 11+ regions. The data model moved to `org.tangled.*` lexicons.

**Repo-owned data.** *Knot2*, a Rust rewrite of the knot server, bundles a custom Git server and SSH server in one binary. Issues, pull requests, comments, reactions, labels, collaborators and the blocklist now live on the knot as collaborative objects: chains of Git objects inside the repo. To stop a knot from forging your words, each edit also writes an attestation record to your own PDS, giving two-way verification. Self-hosters must upgrade to knot v1.20.0.

**Features.** Organization accounts are real AT Protocol accounts (passwordless, delegated, hosted on a Tranquil PDS) with `<name>.tngl.sh` handles and invitable members. The PR flow is overhauled: everyone, with Git or Jujutsu, can submit and review stacked PRs, with interdiffs between versions (and between individual changes for JJ users).

**Next.** The launch is staggered over weeks: private repos, a CLI, language SDKs and agent skills are coming.
