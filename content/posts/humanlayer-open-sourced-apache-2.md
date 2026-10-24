---
title: "HumanLayer open-sources its Claude Code and Codex session manager under Apache 2.0"
slug: "humanlayer-open-sourced-apache-2"
date: 2026-10-24T08:05:00+0000
summary: "HumanLayer has released the code behind its cloud product under Apache 2.0: the desktop app, the daemon that runs Claude Code and Codex sessions, the cloud API, and its integrations. Self-hosting is possible via docker compose today, but many third-party API keys are still required."
source: "https://www.hlyr.dev/blog/open-sourcing-humanlayer"
source_title: "Open sourcing HumanLayer"
source_author: "Dex"
source_site: "hlyr.dev"
source_date: "2026-10-23"
hn_url: "https://news.ycombinator.com/item?id=50215925"
---

**Bottom line:** HumanLayer's core, the same code its hosted cloud runs, is now open source under Apache 2.0. The company argues that "software factories" built from agents must be open and composable, not a black box in the middle of the SDLC.

- **What's included.** The desktop app (Electron, plus a Tauri build) and web UI; the daemon that runs Claude Code and Codex sessions on your machines, shipped in the `humanlayer` CLI; the cloud API and the Electric proxy that syncs Postgres to clients; the humanlayer-pi and humanlayer-opencode plugins; the RPI skills and Linear CLI; and the data model (orgs, teams, projects, tasks, sessions, plans, diffs, PRs) with GitHub, Linear, Jira and Slack integrations.
- **Telemetry.** None if you build and run it yourself; data is sent only if you set keys or DSNs.
- **Self-hosting.** A docker compose devstack exists, but several third-party keys (WorkOS, Slack, Linear and others) are currently required for processes to start. Making them optional is the first planned fix. AWS, IaC and Kubernetes deployment options are planned.
- **Existing users.** Cloud-hosted HumanLayer is unchanged for now.
- **Risks.** The authors say open source brings maintainer-triage and security burdens, citing "events of this week", and say they hardened the app beforehand.
- **Rationale.** They cite David Crawshaw's "Devtools must be open source": once an agent can read and patch a tool's source, closed tools are frustrating because you can't fix or customize them.
