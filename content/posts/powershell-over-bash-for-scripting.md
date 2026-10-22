---
title: "PowerShell 7 beats Bash and Node for cross-platform scripting, one dev finds"
slug: "powershell-over-bash-for-scripting"
date: 2026-10-22T06:03:06+0000
summary: "Corbin Davenport argues that PowerShell 7, the cross-platform .NET version, is a better scripting tool than Bash for anything beyond a list of commands, and that he now reaches for it instead of Bash, Node.js or Python even on Mac and Linux. Built-in argument parsing, HTTP requests, JSON handling and real data types remove the boilerplate and external dependencies (curl, jq) that make Bash scripts fragile on other people's machines."
source: "https://www.spacebar.news/powershell-is-actually-good/"
source_title: "I can't believe PowerShell is actually good"
source_author: "Corbin Davenport"
source_site: "Spacebar"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50190959"
---

The author's claim is that PowerShell 7 (the cross-platform, .NET Core based version, installable on Windows, macOS and Linux, not the legacy Windows PowerShell 5.1) is more predictable and reusable than Bash for non-trivial scripts.

**Why Bash falls short for him:** almost no built-ins, so scripts lean on external tools (curl or wget, jq, clipboard utilities) that may not exist on another person's machine; option parsing needs boilerplate; behavior differs across shells. Node.js needs await everywhere and third-party dependencies for simple jobs like unzipping, and he dislikes Python's lack of curly braces.

**What PowerShell gives him:**
- A `Param` block gives mandatory, typed parameters, prompts and `Get-Help` output with no hand-written parsing.
- Native integers, strings, arrays, hash tables and objects, plus .NET calls when needed.
- `Invoke-WebRequest`, `ConvertFrom-Json` and `ConvertTo-Json` are built in, covering the curl/jq jobs.
- Providers expose things like the Windows Registry as virtual drives, so `Test-Path HKLM:\...` works like a file check.
- Unix-style paths such as `~/Desktop` work on all platforms.

**Evidence:** he used it for the Windows installer of Just the Browser (registry edits, downloads, a menu of per-browser actions) and for a Tumblr-to-Eleventy migration script that pulls from the Tumblr API and an archive ZIP and emits metadata JSON. He says Node would have needed extra dependencies for the latter.

Caveats he notes: Windows 11 still ships only the old Windows PowerShell, so PowerShell 7 must be installed separately, and native commands may not convert Windows-style paths automatically. This is one developer's experience, not a benchmark.
