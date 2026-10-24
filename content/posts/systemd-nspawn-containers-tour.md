---
title: "systemd-nspawn as chroot-on-steroids: a 19-minute setup tour"
slug: "systemd-nspawn-containers-tour"
date: 2026-10-24T00:02:31+0000
summary: "systemd-nspawn is a namespace-based Linux container tool that, unlike Docker, runs a writable root filesystem much like a lightweight VM. The author uses it to isolate self-hosted LLMs while sharing the GPU, and walks through building rootfses, booting, managing with machinectl, and networking."
source: "https://quantum5.ca/2025/03/22/whirlwind-tour-of-systemd-nspawn-containers/"
source_title: "A whirlwind tour of systemd-nspawn containers"
source_author: "Guanzhong Chen"
source_site: "quantum5.ca"
source_date: "2025-03-22"
hn_url: "https://news.ycombinator.com/item?id=50214983"
---

systemd-nspawn is a lightweight, namespace-based container tool that behaves more like a VM than Docker: there are no Dockerfiles or prebuilt read-only images, just a writable root filesystem in a directory (a "chroot on steroids"). The author recommends it for isolation on a VPS without nested virtualization, for sharing hardware such as a GPU (their reason for running Ollama and Open WebUI in it), for avoiding virtualization overhead, and for direct access to host files.

## Building a rootfs

Containers live under `/var/lib/machines/<name>`, and you build the rootfs with your distro's tooling:

- **Debian/Ubuntu:** `debootstrap`, including systemd, dbus, locales and curl. Result: about 338 MB.
- **Fedora:** `dnf --installroot`, about 507 MB, of which roughly 260 MiB is dnf cache.
- **Alpine:** about 16 MB. The usual `alpine-chroot-install` bind-mounts `/home`, `/dev`, `/proc` and `/sys` into the chroot, which nearly cost the author their home directory and could chown host devices. They forked it as `alpine-nspawn-install`, which drops those behaviors and sets up OpenRC and sshd.

## Running it

- A first shell: `systemd-nspawn -D /var/lib/machines/<name> -M <name> --private-users=pick --private-users-ownership=chown`. This gives a 65536-ID user namespace and chowns the rootfs to match, and later boots use `--private-users=yes`. It is only a root shell with no init.
- For a real deployment, put `PrivateUsers=yes` in `/etc/systemd/nspawn/<name>.nspawn`, then use `machinectl start`, `login`, `shell` and `enable` (start on boot). For non-systemd distros such as Alpine, add `KillSignal=SIGTERM`, and use sshd because `machinectl login` needs systemd inside.

## Networking

The default is host networking with no isolation. Alternatives are a Linux bridge (`Bridge=br_test`, with the container's `host0` configured by DHCP or static IPs) for setups where the container should sit on the real network, and a NAT setup with a private bridge for hosts with a single IP.

The post goes on to cover other common scenarios such as bind mounts and resource limits.
