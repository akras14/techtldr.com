---
title: "FreshRSS on NixOS took 9 lines of Nix, plus backups"
slug: "nixos-freshrss-nine-lines"
date: 2026-10-20T14:04:53+0000
summary: "The author says NixOS removed their fear of trying new self-hosted software: a working FreshRSS install with backups took 9 lines of Nix (plus about a dozen for the nginx reverse proxy), has run hands-off for months, and could be rebuilt on a fresh server in roughly 15 minutes."
source: "https://entropicthoughts.com/nixos-is-making-me-less-afraid"
source_title: "NixOS is making me less afraid"
source_author: "kqr"
source_site: "entropicthoughts.com"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50165484"
---

NixOS lowered the cost of trying and maintaining self-hosted software for the author. A working FreshRSS configuration, backups included, took 9 lines of Nix, plus about a dozen more for the nginx reverse proxy. It has needed no attention for months, and if the server dies the same configuration can bring up a replacement in about 15 minutes.

**Why they wanted it.** Their RSS feeds lived only on a phone, and read/unread state was lost when they changed phones. A server-side aggregator was the fix, but they had avoided it because it would be one more side project to maintain.

**The non-Nix alternative.** Installing FreshRSS by hand means extracting an archive, setting permissions, installing nginx and php-fpm, configuring pools, setting up Let's Encrypt and backups, and remembering all of it later. The container image shortens this but still leaves backups, a systemd unit, and a TLS-terminating proxy, and nothing guarantees the same image works six months later unless you recorded exactly which one you pulled. Rebuilding from a Dockerfile doesn't help, since builds separated in time can differ.

**Versus other tools.** The author tried CFEngine, Ansible, podman, and k3s without a real improvement over written notes or a shell script of ssh commands. Those scripts rot as assumptions change. A NixOS configuration reads like those careful notes, but it is executable and gives the same result each time.

The piece is one person's experience, not a comparison or benchmark.
