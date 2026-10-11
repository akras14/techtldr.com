---
title: "Texas's data center queue hit 474 GW, so the grid operator froze permits"
slug: "texas-data-center-permit-freeze"
date: 2026-10-11T17:02:11+0000
summary: "Texas's grid queue for large new loads grew from 63 GW at the end of 2024 to 474 GW by June 2026 (about 90% data centers), more than five times record peak demand. Regulators have since frozen new approvals, because many requests are speculative duplicates and the grid has no cheap way to tell real projects from fake ones. Bringing your own power only helps as a bridge: nearly every on-site plan still intends to join the grid."
source: "https://a16z.com/why-texas-is-making-data-centers-wait/"
source_title: "Why Texas Is Making Data Centers Wait"
source_author: "Ryan McEntush"
source_site: "Andreessen Horowitz"
source_date: "2026-10-07"
hn_url: "https://news.ycombinator.com/item?id=50043603"
---

**Bottom line:** Texas grid operator ERCOT went from 63 GW of large new customers in its queue (end of 2024) to 474 GW by June 2026, mostly data centers and over five times the state's record peak demand. Texas then paused new approvals. The author, an a16z partner, says the freeze is about queue quality and community trust as much as raw power.

**Why the pause**
- Developers file the same project at several sites, and many are speculative builds with no customer. Planners can't tell which are real, and don't want ratepayers funding upgrades for projects that never appear.
- Locals want straight answers on noise, water, emissions and bills. Only 28 of 377 companies had answered a state resource-use survey, and one county was asked to approve a tax waiver for an unnamed "Project Patriot".

**Why grid connection got hard**
- Spare network capacity is gone, so nearly any new large load needs upgrades. Buying power from a plant on paper no longer guarantees a path to your building.
- Co-locating next to a plant is contested: federal regulators rejected a direct-supply deal at Amazon's Susquehanna campus, and Texas approvals for sites like Freestone and Armstrong County require the load to shed or switch to backup within 30 minutes of a grid call.
- Studies moved to batches ("Batch Zero", loads of 75 MW or more), but that is stalled pending an audit due in December, including 17 projects that had cleared every other step.
- Data centers that switch to backup at the first voltage dip can drop off the grid all at once, so new ones must now ride through routine faults. Some labs run dummy jobs after a failed training run to avoid an abrupt load drop.
- Commitments are cheap: a $100,000 study fee plus $50,000 per MW deposit, with only 20% forfeited if a project loses its capacity.

**Bring-your-own-power is a bridge, not an escape**
- Examples include gas at Abilene, Shackelford County and West Texas, and a wind-farm-tied Google campus. SemiAnalysis tracks 75 GW of behind-the-meter equipment orders, yet nearly every site plans to connect to the grid when it can, because on-site power costs more.
- Islands must supply inertia, steady voltage and backup themselves. At xAI's Memphis site, 10 to 20 MW swings several times a second wore out turbine shafts until Tesla Megapacks were added. Some buyers accept designs with about 99% uptime, since waiting years for a connection costs more.
- Texas limits private power networks. Utah's SB 132 lets 100 MW-plus loads go fully off-grid, and the author expects permissive states to win more of the buildout.
