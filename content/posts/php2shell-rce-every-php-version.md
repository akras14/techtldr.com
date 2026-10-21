---
title: "PHP2Shell: four PHP engine bugs give unauthenticated RCE back to PHP 5.4"
slug: "php2shell-rce-every-php-version"
date: 2026-10-21T15:04:01+0000
summary: "Wiz Research disclosed PHP2Shell, four memory-safety bugs in PHP's core engine (under CVE-2026-109618 and CVE-2026-109619, up to CVSS 9.2) that allow unauthenticated memory disclosure and remote code execution. At least one is present in every PHP release back to 5.4, and Wiz demonstrated RCE against the latest WordPress, Drupal and Joomla container images. Patch to the October 21 PHP security release."
source: "https://www.wiz.io/blog/php2shell-php-rce"
source_title: "PHP2Shell: AI uncovers critical RCE in PHP"
source_author: "Wiz Research"
source_site: "Wiz"
source_date: "2026-10-21"
hn_url: "https://news.ycombinator.com/item?id=50180836"
---

This is the detail behind the [PHP security release](/php-critical-security-release-oct-21/) announced ahead of time. Wiz Research found four memory-safety bugs in PHP's core engine, rooted in mishandled fatal errors (especially out-of-memory errors) that leave corrupted state for a later request or worker to reuse. The only complete fix is the PHP security update released October 21, 2026.

- **CVE-2026-109618:** an upload-progress unserializer use-after-free plus a remote process-memory disclosure bug. Together they give unauthenticated memory disclosure and, chained, RCE.
- **CVE-2026-109619:** a fatal-error backtrace use-after-free and an OPcache type confusion, both used to build RCE chains.
- **Reach:** at least one bug exists in every PHP release of the last decade, back to 5.4. Because the flaws are in PHP itself, secure application code doesn't protect you.
- **Demonstrated impact:** unauthenticated RCE against the latest WordPress, Drupal and Joomla containers. Attackers could also read worker memory (passwords, tokens, API keys, certificates) without code execution.
- **AI-assisted:** Wiz says it used Gemini 4 Argon, GPT-5.6 Sol and its own tooling. The first bug was found August 19 and RCE confirmed on Drupal and WordPress within two days. The authors say LLMs helped adapt exploits to different PHP versions, configurations and memory layouts.
- **Variants:** Wiz expects other variants exist, but says the patch fixes the root cause.

If you can't patch immediately, Cloudflare, AWS, Akamai, Fastly, Google, Microsoft and others published mitigations. Signs to watch for: unusual multipart requests followed by memory-limit errors, repeated HTTP 500s, oversized or malformed `PHPSESSID` cookies, and unexplained PHP worker churn.
