---
title: "Prospero: a C++ coding harness using about 100x fewer resources"
slug: "prospero-coding-harness"
date: 2026-10-31T14:02:35+0000
summary: "Chris Wellons released Prospero, an open source coding harness written in C++ with a Dear ImGui interface. He argues a harness is essentially curl glued to popen(), so it shouldn't need gigabytes of RAM, and claims it uses about two orders of magnitude fewer resources than web-stack harnesses without dropping features. It has no built-in model provider (bring llama.cpp, OpenRouter, or the Anthropic or OpenAI APIs; Claude subscriptions don't work), a headless engine that can run over SSH or WSL, and desktop control for vision models."
source: "https://nullprogram.com/blog/2026/10/31/"
source_title: "Prospero: A new coding harness for a more civilized age"
source_author: "Chris Wellons"
source_site: "null program"
source_date: "2026-10-31"
hn_url: "https://news.ycombinator.com/item?id=50315226"
---

**Bottom line:** Chris Wellons built Prospero because the coding harnesses he tried this year were bloated web-stack apps. His view is that a harness is little more than `curl` glued to `popen()`. Prospero is a C++ app with a Dear ImGui UI that he says uses roughly two orders of magnitude fewer resources than the alternatives while keeping their features.

## Features

- **Bring your own model:** No provider is built in. Point it at llama.cpp, an on-prem model, OpenRouter, or the Anthropic or OpenAI APIs. Claude subscriptions can't be used.
- **Headless engine:** The engine can run on a remote or headless machine (over SSH or WSL) while the UI stays on your desktop, and it keeps running without the GUI. Embedded engines cover x86/x86-64 and ARM64 on macOS, Windows, and Linux; the Windows x86 engine reportedly runs on XP.
- **Desktop control:** With a vision-capable model the agent can click, type, and drag to test GUI apps.
- **Small footprint:** Idle rendering uses no GPU. Wellons suggests it suits fuzzing campaigns, where the Linux OOM killer is less likely to kill the harness instead of the runaway target.
- **Extras:** Git worktrees (including for subagents), MCP support, optional web search, side chats, session forking, and a built-in skill that answers questions about Prospero and edits its INI config.

## Design principles

- **Never wait forever:** Every operation has a deadline. Unanswered permission requests time out as denials, and shell commands that time out continue as background tasks the agent checks on. Plan mode is the exception.
- **No stray processes:** A process tree lives and dies together (job objects on Windows; harder on macOS and Linux, but implemented).
- **Permissions aren't security:** They are guardrails for simple mistakes, not a sandbox. For real isolation, run the harness in a container or on separate hardware.

## Editions

A free Community Edition on GitHub (no bundled headless engines, with a nag message) and a Professional Edition for a nominal fee, DRM-free with free updates and all headless engines built in.
