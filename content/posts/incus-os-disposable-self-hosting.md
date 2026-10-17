---
title: "Self-hosting 20 services on one laptop with Incus OS, no SSH, rebuilt from git"
slug: "incus-os-disposable-self-hosting"
date: 2026-10-17T07:02:30+0000
summary: "Solène Rapenne spent seven months rebuilding her home infrastructure on a single 16 GB laptop running Incus OS, an immutable, shell-less system that only runs Incus. About twenty Ubuntu system containers and two VMs are declared in git with OpenTofu and cloud-init, and any change destroys and recreates the affected instance, so there is no configuration drift. She applies SecNumCloud-style principles at home, and says it is probably overengineered for most people."
source: "https://dataswamp.org/~solene/2026-10-15-selfhosting-infrastructure-incus-os.html"
source_title: "My self-hosted infrastructure with Incus OS"
source_author: "Solène Rapenne"
source_site: "dataswamp.org"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50124646"
---

**Bottom line:** a single home laptop runs about twenty services (Nextcloud, Forgejo with CI, Immich, PeerTube, Vaultwarden, a Prometheus/Grafana/Loki stack and more) on Incus OS, with no SSH and everything rebuilt from git. The author calls it overengineered for most people, but the design is a useful template for disposable, deny-by-default infrastructure.

- **Base system:** Incus OS is immutable, with Secure Boot, A/B updates, TPM disk encryption and no local shell, SSH or package manager. It is managed over the Incus API with a client certificate, so there are no SSH keys to manage.
- **Disposable instances:** each service runs in an unprivileged Ubuntu system container (two sensitive parts are VMs: the haproxy edge and the CI runner), with apps as podman quadlets inside. OpenTofu and cloud-init rebuild an instance from scratch on any change. Only data volumes persist. The cost is one to two minutes of downtime per change.
- **Network:** Incus ACLs deny everything by default, including between instances; every allowed flow is declared in code. Outbound traffic goes through one exit instance with a domain allowlist, plus apt and container-registry caches.
- **Ingress:** haproxy forwards raw TCP with the PROXY protocol and holds no TLS keys. CrowdSec drops bad IPs in nftables, and caddy terminates TLS. A second caddy serves VPN-only tools and admin APIs, using certificates from a private ACME CA.
- **Secrets, updates, backups:** secrets are encrypted in git with sops and age, one key per instance. Updates are automatic (Incus OS, unattended-upgrades, podman auto-update pinned to major tags). Backups are ZFS snapshots plus client-side-encrypted restic to versioned S3.
- **Limits:** a single machine at home means no availability guarantee. The author also lists CrowdSec setup, multi-layer proxy headers and Incus ACL semantics as the hard parts.
