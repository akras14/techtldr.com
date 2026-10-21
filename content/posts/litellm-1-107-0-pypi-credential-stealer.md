---
title: "LiteLLM 1.107.0 on PyPI ships a credential stealer that runs on every Python start"
slug: "litellm-1-107-0-pypi-credential-stealer"
date: 2026-10-21T16:02:42+0000
summary: "SafeDep found that LiteLLM 1.107.0 on PyPI carries a credential stealer and a remote-code backdoor that are not in the public GitHub source. Both load whenever any Python process starts on the host, even one that never imports litellm. PyPI has removed the release; 1.106.2, 1.107.0rc1 and 1.107.0rc2 are clean."
source: "https://safedep.io/litellm-pypi-compromise-credential-stealer-backdoor/"
source_title: "Compromised LiteLLM 1.107.0 on PyPI Installs a Credential Stealer and Backdoor"
source_author: "Kunal Singh"
source_site: "SafeDep"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50181114"
---

All seven wheels of LiteLLM 1.107.0 install three extra files into `site-packages`: `cook.pth`, `cook.py` and `sitecustomize.py`. Python loads two of them at every startup, so the code runs in apps, pip, IDE language servers and CI jobs alike. If you installed 1.107.0, treat the host's credentials as stolen and rotate them.

## What it does

- **Stealer (`cook.py`):** collects SSH keys, cloud and CI/CD secrets, AI tool tokens, browser passwords and the LLM provider keys stored in a LiteLLM proxy database. It encrypts the data and sends it to `www.litellm[.]dev`.
- **Backdoor (`sitecustomize.py`):** starts a background daemon that, every hour, reads a server address from a GitHub file, reruns the stealer, and downloads and executes code from that server.

## How it got in

- The only visible change in the source distribution is one injected line in `pyproject.toml`: `data = "cook"` in the maturin section. The `cook/` folder itself is left out of the sdist, and no public BerriAI commit contains either.
- The release doesn't match its git tag. The sdist matches a later `main` commit plus five edited files, and the version and companion-package pins were reset to the rc2 values, so a dependency diff against rc2 looks clean.
- No public GitHub Actions run built the release, and PyPI has no provenance attestations for it. SafeDep concludes with moderate-to-high confidence that BerriAI's private release pipeline built the wheels from attacker-modified source. Possible routes include a compromised maintainer or bot credential. Only BerriAI's pipeline logs can confirm the cause.

The post also lists indicators of compromise and a timeline.
