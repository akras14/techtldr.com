---
title: "Tailscale 1.104.2 and headscale 0.29.5: 11 findings, from SSH socket forwarding to root"
slug: "tailscale-ssh-root-socket-flaws"
date: 2026-10-25T15:02:11+0000
summary: "An independent audit of Tailscale 1.104.2 and headscale 0.29.5, published without prior vendor disclosure, reports 11 findings. The most serious: Tailscale SSH's unix-socket forwarding runs as root, so a session from any policy-permitted device can take over privileged sockets and reach root on the target. These are the researchers' lab results, not vendor-confirmed."
source: "https://tailworm.com/"
source_title: "Tailworm: Security Assessment Report — Tailscale v1.104.2 and headscale v0.29.5"
source_author: "Erica Windisch, Cyberdione Labs"
source_site: "tailworm.com"
source_date: "2026-10-24"
hn_url: "https://news.ycombinator.com/item?id=50227138"
---

An independent research team (Cyberdione Labs, not affiliated with or paid by Tailscale) published an audit of Tailscale v1.104.2 and headscale v0.29.5 covering 11 findings, most rated High. They chose to publish immediately rather than follow a disclosure timeline, arguing that infrastructure is vulnerable today. All results come from lab reproductions on unmodified releases (tested 2026-10-22 to 10-24); nothing here is vendor-confirmed, and fixes may land after this writeup.

## The main finding: Tailscale SSH socket forwarding

- **Setup:** Tailscale SSH's embedded server (running as root inside `tailscaled`) does no user-level authentication. The WireGuard-identified device plus one SSH policy rule is the whole authorization.
- **Flaw:** unix-socket forwarding (`ssh -L` / `ssh -R`) does path checks and file operations as root, and the path allowlist is subject to TOCTOU races. An unprivileged session can dial, replace or delete root-owned sockets (container runtime, init system) and capture device-enrollment keys.
- **Requirement:** one approved device and one SSH rule that permits forwarding. On headscale, forwarding is granted unconditionally and can't be turned off.
- **Spread:** in the lab, the researchers automated hops across every device the policy allowed, under a second per hop. Per-pair access grants were the only control that limited it.

## Other findings

- **App connectors (conn25, experimental):** the connector doesn't validate client-chosen destinations against the app's configuration and skips ACL checks, so an entitled device can reach cloud metadata endpoints, internal LANs, or every Kubernetes service.
- **headscale:** unauthenticated memory exhaustion and cache eviction can crash the control server and block new devices from enrolling.
- **DERP client:** an unbounded read from a malicious relay can exhaust agent memory.
- **`tailscale serve`:** a TCP forward to the local API socket lacks a guard that exists on the HTTP path, exposing the admin API to peers. A separate race can bypass per-mount authorization.

## Mitigations the report recommends

1. Use per-pair SSH grants, never identity-wide or tag-wide rules.
2. Turn off SSH forwarding in each rule on SaaS. On headscale 0.29.5, treat every SSH grant as root-equivalent for now.
3. Set node-key expiry on server fleets and use TPM- or Secure-Enclave-bound agent state.
4. Restrict connector entitlements and add default-deny egress on connector namespaces.
5. Respond to incidents by deleting the device at the control plane; reimaging a host doesn't revoke a stolen node key.
6. Self-hosted headscale: set memory limits and rate-limit registration.

**Also covered by:** [Tailscale, "Tailscale SSH: Investigation and remediation update"](https://tailscale.com/ssh-investigation-update) (Oct 26; [HN](https://news.ycombinator.com/item?id=50264857)). The vendor confirms "serious" Tailscale SSH privilege-escalation issues: someone already authorized to connect may exceed their configured local user's permissions. Fixes for most issues are implemented, with a patched client targeted for the end of Oct 27, and a security advisory and technical post to follow. Tailscale does not recommend disabling Tailscale SSH outright; interim advice is to tighten SSH access policies and set `TS_SSH_DISABLE_FORWARDING=1` in the tailscaled environment, which it says mitigates several of the more serious findings.
