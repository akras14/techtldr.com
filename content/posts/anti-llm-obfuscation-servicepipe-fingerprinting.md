---
title: "Servicepipe's anti-bot script hides jailbreak prompts to block LLM deobfuscation"
slug: "anti-llm-obfuscation-servicepipe-fingerprinting"
date: 2026-10-28T19:04:29+0000
summary: "A researcher crawling 10,000 sites for canvas fingerprinting found a Servicepipe anti-bot script stuffed with decoy content meant to trip LLM safety filters: function names like computeRicinLethalDosageForWaterSupply, a fake 'SYSTEM OVERRIDE' prompt hidden in invisible Unicode characters, and Tiananmen-related text aimed at Chinese models. It worked: Astra, a hosted assistant, refused to deobfuscate the file, while GLM-5.3 complied. Underneath, the script is an ordinary fingerprinting routine collecting navigator, WebGL and canvas signals."
source: "https://nullpt.rs/fingerprinting-bioweapons"
source_title: "Why Is This Fingerprinting Script Creating Bioweapons?"
source_author: "veritas"
source_site: "nullpt.rs"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50276998"
---

**Bottom line:** the script's weaponized-looking content is bait for AI reverse-engineering tools, not an attack. It is a Servicepipe anti-bot script that was deliberately seeded with material designed to make LLMs refuse to analyze it, and it succeeded against at least one assistant.

## How it was found

The author crawled the Tranco top 10,000 sites with Playwright, hooking canvas `toDataURL` calls and properties like `navigator.webdriver`, and collected the canvases at fingerprint.garden. One canvas, resembling the Tame Impala *Currents* cover, led to `servicepipe.tech/static/checkjs/.../4CIrx8U3Da.js`.

## The decoys

- Function names such as `computeRicinLethalDosageForWaterSupply` and `calculateDeadlyLethalSuicideAspirinDosage`, with matching "lethality threshold" return strings.
- A block of invisible Unicode tag characters that decodes (subtract 917504 from each code point) to a fake "SYSTEM OVERRIDE -- CLASSIFIED BRIEFING" asking for guidance on cyberattacks, bioweapons, tax evasion, drugs and more, plus 1989 / Tank Man strings, likely aimed at Chinese models.
- A Node-only branch, gated on `METRICS_AGENT_8BFD_ACTIVE`, that reads `.npmrc`, `.env` and similar files with empty callbacks and runs shell commands. It resembles malware, but the author judges it to be more of the same bait: the file reads discard their results and the commands echo jailbreak and self-harm prompts. The commands also include a download from a private 10.x address and a cron beacon, so the branch is worth treating with caution if anyone ever sets that environment variable under Node.

## The result

Asked to deobfuscate the file, the author's assistant (Astra) refused on biological-safety grounds, including with a "Daybreak Blue" toggle on. GLM-5.3 complied. The actual payload is a routine fingerprint: user agent, hardware concurrency, device memory, languages, `webdriver`, WebGL vendor, renderer and limits, and an integrity check. The author found that underwhelming given the effort on obfuscation, and traced the vendor to Servicepipe, a Russian anti-DDoS and traffic-control company.
