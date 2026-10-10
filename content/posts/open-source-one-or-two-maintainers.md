---
title: "11 of 23 Core Open Source Projects Run on One or Two People"
slug: "open-source-one-or-two-maintainers"
date: 2026-10-10T02:02:56+0000
summary: "A Reddit analysis of 23 foundational open source projects found that 11 had only one or two regular contributors (ten or more changes) in the year to 7 October 2026. Eight of the 23 show no grant or sponsorship in the public sources checked, and funding tends to arrive after disasters rather than before them."
source: "https://linuxstans.com/11-of-23-core-open-source-projects-run-on-1-or-2-people/"
source_title: "11 of 23 Core Open Source Projects Run on 1 or 2 People"
source_site: "Linuxstans"
source_date: "2026-10-09"
---

**Bottom line:** A Reddit user (u/Mastbubbles) pulled the full history of 23 projects that phones, browsers and servers depend on and counted everyone with ten or more changes between 7 October 2025 and 7 October 2026. In 11 of the 23, that was one or two people. Eight projects show no grant or sponsorship from the Sovereign Tech Agency, Alpha-Omega, Open Collective or GitHub Sponsors, though that is a narrow test and doesn't mean nobody has ever paid these people.

## Examples

- **xz:** one regular contributor, Lasse Collin, who wrote 97% of its 2025 changes. The analysis found no new funding after the 2024 backdoor.
- **Time zone database:** Paul Eggert (UCLA) made 218 of 251 changes, with Tim Parenti as backup. Android, iOS and most servers read this file.
- **sudo:** Todd Miller made 5,408 of 5,409 changes from 2008 to 2018. After he publicly asked for a sponsor in February 2026, funding rose to roughly 1,700 a year plus 30 GitHub sponsors, making it the best-funded one-person project on the list.
- **bash:** Chet Ramey, alongside a university day job, with his name on every change in the public history.
- **core-js:** raised about $57 a month in donations; its author wrote 95% of this year's changes.
- **zlib, libjpeg-turbo, HarfBuzz, SQLite:** tiny teams behind code on billions of devices. SQLite pays for its work by selling support.

## Money follows disasters

After Heartbleed in 2014, OpenSSL (living on about $2,000 a year) got $5.4 million via the Linux Foundation, paid developers and an audit; it now has 32 regular contributors. After the xz backdoor, the analysis found nothing comparable. curl, with 11 regular contributors, is funded through company support contracts, Open Collective, GitHub sponsors and a €195,000 Sovereign Tech Agency grant. libxml2 shows a successful handover: a new maintainer was in place about twelve hours after the previous one stepped down.

Public funders mostly give to organizations that can apply and report, which favors projects with an institution behind them over a lone maintainer.

## Caveats and discussion

"No public grant" is a narrow measure, and several maintainers have day jobs. In the r/linux thread, commenters were angry at companies that ship these libraries without paying, disagreed on whether distro patching reduces the risk of a lone upstream maintainer, and split on whether xz showed open review working or luck (it was caught by one engineer chasing slow SSH logins).
