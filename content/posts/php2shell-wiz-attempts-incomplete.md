---
title: "PHP2Shell: Wiz saw no completed exploits; exposed machines down 67%"
slug: "php2shell-wiz-attempts-incomplete"
date: 2026-10-27T15:04:08+0000
summary: "Wiz's honeypots have recorded PHP2Shell exploitation attempts since the October 21 disclosure, but none completed the exploit chain, and Wiz found no public PoC that works generally against arbitrary targets. Daily internet-exposed vulnerable machines fell about 67% in a week. Wiz still rates the risk as real in the short and long term and says to patch."
source: "https://www.wiz.io/blog/php2shell-exploitation-so-far"
source_title: "PHP2Shell: Exploitation Attempts and Remediation Trends"
source_author: "Yaara Shriki, Shahar Dorfman, Alma Blachsan, Yaron Samuel"
source_site: "Wiz Blog"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50259343"
---

**Bottom line:** A week after disclosure of the two PHP bugs (CVE-2026-109618 and CVE-2026-109619), Wiz has seen a limited number of exploitation attempts in its telemetry and none that completed the exploit chain. It has found no public proof of concept that works end to end against an arbitrary target. Wiz still calls PHP2Shell a risk in the short and long term, and patching remains essential.

## Exposure is dropping
- Wiz telemetry shows roughly a 67% drop in daily distinct internet-exposed vulnerable machines since October 21.
- Affected organizations (at least one exposed, vulnerable instance) fell about 47.9%; under 53% of the initially affected remain vulnerable.
- Among vulnerable hosts, the share running PHP 7.3 or earlier (no official backports) went from 8.3% to 5.7%.
- Wiz can't say how much of this is patching versus retiring machines or restricting access.

## What the attempts looked like
- **Legacy chain:** Needs a ~12 MB multipart body; it failed on the honeypot because the request exceeded the 8 MB `post_max_size`. A later attempt against a Roundcube honeypot never reached memory exhaustion either.
- **Current chain:** Reliably exhausted PHP's memory (stage A1), but the follow-up memory-leak stage (B1) failed in every captured run, so no stage-three exploit was sent. The attackers also tie up other Apache workers with stalled requests so consecutive requests hit the same process.
- **Why it fails:** The kit's heap layout was calculated for the official `php:8.4.26-apache` image with a 128 MB limit. Different builds, memory limits, request timeouts and network latency all broke it.
- **Mislabeled tools:** Some "PHP2Shell" tools don't touch the PHP bugs at all; they use session poisoning plus a separate local file inclusion bug that the PoC's own lab app supplies.

## Caveats
Wiz points to Adel Karimi's public honeypot report as the strongest public evidence of real exploitation (traffic, memory disclosure, then code execution). That target was a favorable single-worker Apache mod_php setup, so it doesn't show reliability elsewhere, and it likely contributed to CISA adding PHP2Shell to its KEV list. Wiz expects the barriers to shrink as capable actors adapt the chain.

Earlier coverage: [exploited four days before disclosure](/php2shell-exploited-four-days-before-disclosure/).
