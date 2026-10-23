---
title: "What building a container runtime taught about Docker's internals"
slug: "docker-is-weird-own-container-runtime"
date: 2026-10-23T01:02:06+0000
summary: "While writing his own container runtime for fun, the author found that containers are ordinary Linux processes restricted by namespaces, cgroups and network isolation, and that images are just folders that Docker chroots (or pivot_roots) into. He also notes that runc has to work around Go's multi-threading and missing fork, and that Docker's root requirement is easy to miss on macOS."
source: "https://codeandcake.dev/posts/2026-10-22-docker-is-weird"
source_title: "Docker is weird"
source_site: "codeandcake.dev"
source_date: "2026-10-22"
hn_url: "https://news.ycombinator.com/item?id=50202681"
---

**Bottom line:** a short, informal post from someone early in building their own container runtime. The main lesson is that Docker is much less magic than it looks, and that writing your own version of a tool you use is a good way to learn it.

- **Containers aren't real.** A container is a normal process on the host kernel, visible in `/proc`. There is no single "container" feature; it is several kernel features combined: namespaces (what the process can see), cgroups (resource limits) and network isolation.
- **Images are just folders.** `docker run ubuntu /bin/bash` downloads the image, stores it as a directory, then uses `chroot` or `pivot_root` to make that folder the process's root.
- **Go is awkward for this job.** Many of the needed syscalls must run single-threaded, but Go's runtime is multi-threaded. runc therefore has a C module that runs before Go initializes. Go also has no real fork, so runc re-executes `/proc/self/exe` with a special argument to approximate one.
- **Docker needs root.** Easy to miss on macOS, since many kernel APIs historically required root, so the daemon usually runs as root. This is improving, but rootless runtimes have to reimplement the networking stack in userspace to match features.
