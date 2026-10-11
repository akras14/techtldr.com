---
title: "NixOS config can generate VEX statements that suppress unreachable CVEs"
slug: "nixos-vex-reachability"
date: 2026-10-11T23:06:00+0000
summary: "SBOM scanners flag hundreds of CVEs on an OS image, most of which don't apply to your configuration. Because NixOS is declarative, the conditions under which a CVE is not exploitable can be written as Nix code that emits an OpenVEX statement only while those conditions hold. The author shows this suppressing a 2007 OpenSSH CVE in grype and has open-sourced the approach as nixos-vex, which they say is not yet high-trust."
source: "https://blog.kammel.dev/post/nixos_vex/"
source_title: "The Unreasonable Effectiveness of VEX in NixOS"
source_author: "Fabian Kammel"
source_site: "DaTosh Blog"
source_date: "2026-10-11"
hn_url: "https://news.ycombinator.com/item?id=50046864"
---

**Bottom line:** NixOS lets you encode "this CVE doesn't affect my system" as code that is re-evaluated on every build, so VEX (Vulnerability Exploitability eXchange) statements stay correct as the configuration changes. This addresses the flood of CVE triage that sysadmins and AppSec teams face, in the same way that LLM-assisted reports have swamped maintainers.

**Where SBOM scanning falls short:** for a Go program, `syft` plus `grype` reports two High CVEs in `golang.org/x/text`, even though the program never imports the affected packages. `govulncheck` does reachability analysis from the Go vulnerability database and correctly reports zero affected vulnerabilities.

**Why operating systems are harder:** a Debian image with `openssh-server` yields 284 matches. The author triages one, CVE-2007-2768, an unfixed username-enumeration issue that only applies if sshd uses OPIE one-time passwords through PAM. Checking this by hand means confirming that keyboard-interactive auth is disabled and that no `opie` PAM module exists. Repeating that for every CVE, on every config change, is not feasible.

**The NixOS approach:** the same conditions can be written as a Nix expression over the system config: no `opie` package, `KbdInteractiveAuthentication == false`, and no PAM auth rule mentioning `opie`. If they all hold, the config emits an OpenVEX `not_affected` statement (`vulnerable_code_not_present`) for that CVE. Passing it to `grype --vex` moves the match to "ignored". If someone later changes the config so the condition fails, the statement disappears and the CVE shows up again.

**Status:** the author triages their own NixOS machines this way and has open-sourced it as `nixos-vex`, with desktop and server demos. They say not to treat it as a high-trust data source yet and want feedback, with the aim of contributing it back to the Nix ecosystem.
