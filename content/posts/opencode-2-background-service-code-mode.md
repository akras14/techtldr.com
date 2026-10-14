---
title: "OpenCode 2.0 runs one background service and a code mode for MCP"
slug: "opencode-2-background-service-code-mode"
date: 2026-10-14T22:03:04+0000
summary: "OpenCode 2.0 is a major rewrite of the open-source coding agent. The TUI, web UI and desktop app now all talk to a single background service on your machine, with a new API and client package. MCP servers are opted into a \"code mode\" by default, where the agent writes scripts that call tools instead of making one call at a time. The upgrade has breaking changes: 1.0 plugins don't work, and the server API and tui.json config changed."
source: "https://anoma.ly/notes/opencode-2-0/"
source_title: "OpenCode 2.0"
source_author: "Anomaly"
source_site: "anoma.ly"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50094994"
---

OpenCode 2.0 is a major rewrite of the open-source coding agent. The TUI, web UI and desktop app now share one background service, there is a rewritten API, and MCP servers run through a scripting "code mode" by default. Upgrading breaks 1.0 plugins, the server API and the `tui.json` config.

## What changed

- **Single service.** The client/server split that 1.x offered mostly to power users is now the default. One background process runs on your machine and every front end connects to it. The new `@opencode/client` package finds the local service automatically, which suits scheduled scripts. There is also `opencode api` on the CLI, and `/pair` or `opencode pair` to connect other hosts and apps.
- **New API.** Rebuilt from scratch after the old one "grew haphazardly". OpenCode knows its own API and uses it, for example to move the current session into a git worktree.
- **Code mode.** A minimal JavaScript interpreter in `@opencode/codemode` turns MCP servers and OpenAPI endpoints into functions. The agent writes a script that calls a tool, maps over the results and calls another, in a single round trip. All MCP servers are opted in by default, which the authors say lets you add a thousand of them with little context bloat.
- **Background work.** Subagents and shell commands can run in the background, and the agent is notified when they finish. You can start a subagent on a different model, or press ctrl+b to push running work to the background.
- **Plugins and SDK.** Nearly every 2.0 feature is built as an internal plugin (80+), so the plugin API stays maintained. TUI plugins are now stable and everything hot-reloads, so the agent can write a plugin mid-session and use it. `@opencode/sdk` exposes the same code as the CLI so you can build your own variant, for example on Durable Objects.

## Upgrading

Update to the latest 1.x, then run `opencode upgrade --major`. Breaking changes: 1.0 plugins won't work, the server API is entirely different, and `tui.json` moved to json. The authors suggest pointing OpenCode at the post so it can migrate your config.
