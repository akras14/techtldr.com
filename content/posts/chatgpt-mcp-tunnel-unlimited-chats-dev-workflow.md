---
title: "Developer ran parallel coding agents in unlimited ChatGPT chats via MCP tunnel"
slug: "chatgpt-mcp-tunnel-unlimited-chats-dev-workflow"
date: 2026-11-02T03:02:07+0000
summary: "A developer says they did most of their software work in regular ChatGPT conversations instead of Codex, using OpenAI's Secure MCP Tunnel to connect local tools and a pool of parallel chats fed from GitHub issues. With OpenAI announcing that unlimited chats will merge with Codex usage limits, they plan to switch to Claude."
source: "https://chaosmanage.github.io/i-killed-unlimited-chatgpt/"
source_title: "I Am the Reason OpenAI Is Killing Unlimited Chats"
source_site: "chaosmanage.github.io"
hn_url: "https://news.ycombinator.com/item?id=50327416"
---

A developer says they did most of their software work in regular ChatGPT conversations instead of Codex, using OpenAI's Secure MCP Tunnel to connect local tools and a pool of parallel chats fed from GitHub issues. With OpenAI announcing that unlimited chats will merge with Codex usage limits, they plan to switch to Claude.

- **Setup.** A Node.js MCP gateway on their Windows laptop (and a similar one on a VPS) aggregates Desktop Commander (files, terminal, tests) and a computer-use tool (screenshots, mouse, browser testing). OpenAI's tunnel client exposes it to ordinary ChatGPT chats, with no separate paid API calls.
- **Workflow.** A stronger planning model writes a plan and splits it into small GitHub issues with acceptance criteria. Several separate chats on a cheaper, slower model each take an issue, sharing the same MCP tools. Parallel agents on one filesystem need coordination, and some issues needed a stronger model to step in.
- **Extras.** They added inline screenshot display through a custom MCP app viewer and a dashboard showing connection health, CPU/RAM and a live feed of tool calls.
- **Why it ends.** The author says the announced change merges unlimited chats with Codex usage, which removes the trick, and that this was the main thing keeping them on an OpenAI subscription. They say they prefer Claude models and get more usage from a Claude plan, and they insist they broke no rules and doubt their setup influenced OpenAI's decision.

The post is anecdotal and doesn't quantify usage or compare results with Codex.
