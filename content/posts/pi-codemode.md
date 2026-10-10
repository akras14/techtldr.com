---
title: "Pi 1.0 adds MCP through Codemode, a JavaScript sandbox in the harness"
slug: "pi-codemode"
date: 2026-10-10T21:31:50+0000
summary: "Pi 1.0 supports MCP through Codemode: instead of loading tool definitions into context, the agent writes JavaScript that calls tools, runs in a locked-down QuickJS sandbox on the harness side, and returns only what it needs. Armin Ronacher says this isn't a reversal of earlier \"just use scripts\" advice, since MCP has moved toward code, but MCP servers still need structured, consistent output for it to work well."
source: "https://lucumr.pocoo.org/2026/10/6/codemode/"
source_title: "What is Codemode"
source_author: "Armin Ronacher"
source_site: "Armin Ronacher's Thoughts and Writings"
source_date: "2026-10-06"
hn_url: "https://news.ycombinator.com/item?id=49978333"
---

Pi 1.0 supports MCP through Codemode: instead of loading tool definitions into context, the agent writes JavaScript that calls tools, runs in a locked-down QuickJS sandbox on the harness side, and returns only what it needs. Armin Ronacher says this isn't a reversal of earlier "just use scripts" advice, since MCP has moved toward code, but MCP servers still need structured, consistent output for it to work well.

## Why bash isn't enough

Ronacher has long preferred CLIs and bash over custom tools: calls compose easily, and models already understand files. But bash can only compose programs that run in the execution environment. Some capabilities have to live in the harness: reading an image (the payload must be injected into the model protocol), or spawning sub-agents.

## Brains vs. hands

He separates the **harness** (the trusted "brain") from the **execution environment** where bash and tools run (the "hands"). They can sit on different file systems with different trust levels. A sandbox like Gondolin protects the hands, not the brain.

Codemode lets the model orchestrate work on the brain side. In Pi it runs JavaScript in QuickJS inside WASM with no network, no file system, no timers and limited RAM. The only thing the code can do is call more tools. The name comes from Cloudflare.

## What it enables

- **Composition outside the context window.** A normal bash tool call returns only the last 2,000 lines; from Codemode, the full output comes back as structured data. Agents often probe 5 to 10 items, then write a script to process the rest.
- **Concurrency.** `Promise.all` works; Pi caps concurrent tool executions at four and queues the rest.
- **State across calls.** `store()` stashes results in the transcript for a later Codemode call to load.
- **Internal model APIs.** Image generation and classifier models are exposed to Codemode but not as regular tools, where they'd waste context. His examples, all agent-written: generating an image, running sentiment analysis across 100 GitHub issues with the Jev classifier, and a 30-step loop where Jev picks moves to debug a tank game.
- **MCP with progressive discovery.** MCP tools aren't put in context; the agent searches for them from within Codemode.

Codemode is on by default in Pi only when MCP is enabled, or with `"defaultTools": ["+codemode"]`.

## What MCP servers should change

Some servers, like Cloudflare's, run Codemode inside the MCP server, so Pi ends up with "Codemode in Codemode": double JSON escaping, confusing for smaller models, and inner code that can't call outer tools. His wish list for servers:

- Return structured JSON (use `outputSchema`).
- Return consistent shapes, not output that changes with result size, which breaks a probe-then-script workflow.
- Support large binary data without workarounds like pre-signed URLs.
- A way to fan out tool search across multiple servers.

Open problems remain: durability (perhaps snapshotting like workflow engines, or a deterministic language like Starlark), binary data, and smaller models that can't drive this pattern.
