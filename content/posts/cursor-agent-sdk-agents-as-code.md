---
title: "Cursor's Agent SDK defines agents as TypeScript projects, deployed with one command"
slug: "cursor-agent-sdk-agents-as-code"
date: 2026-10-28T15:03:02+0000
summary: "Cursor released its Agent SDK, the framework behind its own managed agents (Bugbot, Security Reviewer, Rollouts), as generally available. An agent is a small code project: Markdown prompts, TypeScript tools, service connections and evals, all under version control. `agent-sdk deploy` builds it, gives it a URL and runs it on Cursor infrastructure, or you can run it yourself as a single Node process in a container."
source: "https://cursor.com/blog/agent-sdk"
source_title: "Introducing the Agent SDK"
source_author: "Rustam Lalkaka"
source_site: "Cursor"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50273731"
---

Cursor's Agent SDK lets teams define event-driven agents in code and manage them like any other software project. It is generally available as of the announcement.

- **Structure:** each agent holds Markdown prompts, TypeScript tool functions, connections to outside services and evals. Changes go through normal version control and review.
- **Triggers:** agents respond to events such as pull requests on GitHub, GitLab or Bitbucket. The code-review example assesses risk, routes the change to the right team and approves changes that meet policy.
- **Multi-agent:** agents can hand tasks to specialized agents or split work among parallel subagents. Evals and event replay are built in.
- **Example:** a Cursor team built `kb-reviewer` in about a day. It checks every PR in their main codebase against a knowledge base of past lessons, flags contradictions and cites the note behind each finding.
- **Deployment:** `agent-sdk deploy` builds the agent and runs it on Cursor infrastructure. Self-hosting is supported, as each agent runs as one Node process that fits in a container.
- **Customer:** PGA America says it builds its software development factory on it, citing the evals.
