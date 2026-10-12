---
title: "A commercial web app in C and CGI: one binary, SQLite, three dependencies"
slug: "c-cgi-web-app-marketyour-games"
date: 2026-10-12T18:02:58+0000
summary: "Corey Prophitt shipped marketyour.games, a commercial product with accounts, Stripe payments and email, as a C program run through CGI on OpenBSD httpd with SQLite. He argues the reasons CGI fell out of favor in the early 2000s no longer hold, and that he would use the stack again. His main complaint is having to write cookie, form and escaping utilities himself."
source: "https://prophitt.me/writing/a-modern-web-app-in-c"
source_title: "A modern web app in c"
source_author: "Corey Prophitt"
source_site: "prophitt.me"
hn_url: "https://news.ycombinator.com/item?id=50058528"
---

**Bottom line:** The author built and sold a modern web product, marketyour.games, using the same model as a 1994 cgi-bin program. The stack is OpenBSD, C, httpd and SQLite ("BCHS"), with no framework, templating language, database server or container. He concludes that CGI's decline was justified in 2004 but not in 2026.

**How it works**
- Every request spawns a fresh process. The statically linked binary reads CGI environment variables and prints HTML directly from C.
- Deploying means replacing one file, with no restart or downtime. Assets are compiled into the binary and served from memory.
- Dependencies are libcurl (Stripe and Postmark calls), libsodium (IDs, password hashing, webhook HMACs) and the vendored libcjson, plus SQLite's amalgamation. Clean builds take a few seconds and rebuilds under half a second.
- OpenBSD's pledge and unveil, plus the chroot httpd runs in, sandbox the binary.

**What he liked:** CGI is just HTTP concepts and its spec hasn't changed since 2004; the setup avoids a second reverse-proxied app server; the supply chain is small; plain Vim and mature tools (gdb, valgrind, sanitizers) are enough; writing HTML in C was less painful than expected; SQLite removes the operational work of Postgres.

**What he'd change:** CGI gives you nothing out of the box, so he wrote cookie, query and form parsing and escaping himself in a header, cgi.h, and isn't sure that was right. He suggests stb-style drop-in utilities and may publish his. If fork cost ever matters, he says FastCGI is a simple upgrade.
