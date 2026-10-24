---
title: "Node.js Brotli decoder bug allows RCE; patch 22.23.4, 24.22.1, 26.12.1"
slug: "nodejs-brotli-decoder-rce-patch"
aliases:
  - "brotli-cve-2026-111497-http2shell"
date: 2026-10-22T14:04:20+0000
summary: "Node.js shipped security releases on October 21, 2026 fixing CVE-2026-111497, a high-severity out-of-bounds write in its bundled Brotli decoder that can leak memory or run arbitrary code. Upgrade to 22.23.4, 24.22.1 or 26.12.1; Node 20 is end of life and gets no fix."
source: "https://vercel.com/kb/bulletin/node-js-security-advisory"
source_title: "Node.js security advisory: Brotli decoder RCE (CVE-2026-111497)"
source_site: "Vercel"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50189292"
---

**Bottom line:** Node.js released security updates on October 21, 2026 for the 22.x, 24.x and 26.x lines. The worst of the ten fixes, CVE-2026-111497, is a high-severity flaw in the Brotli decoder bundled with Node. Upgrade to **22.23.4, 24.22.1 or 26.12.1** now.

- **The bug:** malformed Brotli input triggers an out-of-bounds write during decoding. Researchers demonstrated process-memory disclosure and command execution, running with the permissions of the Node.js process.
- **Who is exposed:** Node doesn't decode Brotli on its own. You're affected when your code or a library decompresses attacker-controlled Brotli data, either in a request body or in a response your server fetches.
- **Other fixes:** the release covers nine more issues, including two high-severity TLS hostname verification bypasses.
- **Node 20:** it is end of life and will not get a fix. Vercel projects on 20.x must move to 22.x or 24.x and redeploy.
- **WAF is not a patch:** Vercel added WAF rules that block the inbound attack vector for all projects, but they don't cover responses your functions fetch from other servers. Self-hosted Next.js, container images, external backends, CI runners and dev machines all need the runtime upgrade. Next.js image optimization does not block Brotli responses.

**Also covered by:** [Aikido Security, "Node2Shell evolves to HTTP2Shell"](https://www.aikido.dev/blog/python-packages-vulnerable-node2shell-brotli-cve-2026-111497) (Oct 23; [HN](https://news.ycombinator.com/item?id=50219177)). The flaw is in Google's Brotli itself (1.0.3 through 1.2.0, CVSS v4.0 9.2), so it also reaches Python's Brotli and brotlicffi (fixed in 1.2.1 and 1.2.1.0), Bun (1.4.4), curl/libcurl, Envoy and Istio, in both directions: client to server and server to client. Python HTTP clients (requests, urllib3, httpx, aiohttp) use Brotli automatically when it is installed, and pip-audit, Dependabot and Safety didn't flag it as of Oct 23. Aikido says OpenAI found the bug in February and it was fixed in source but never flagged as a security issue. Workarounds: uninstall Brotli packages, or reject `Content-Encoding: br` at the proxy and drop `br` from `Accept-Encoding`.
