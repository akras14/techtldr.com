---
title: "China proposes export controls on frontier model weights at 10^26 FLOPs"
slug: "china-ai-export-controls-announcement-48"
date: 2026-11-02T10:02:33+0000
summary: "China's commerce and science ministries proposed adding 23 AI technologies to its export-restricted catalogue (Announcement No. 48, October 16, 2026). Model weights and related know-how become restricted at 10^26 training FLOPs, 2 trillion parameters, or a global top-10 ranking. The catalogue is a draft open for comment until November 15, but an interim pre-export registration requirement is already in effect."
source: "https://labs.cloudsecurityalliance.org/research/csa-research-note-china-ai-export-controls-announcement-48-2/"
source_title: "China's First Frontier AI Export Controls: MOFCOM Announcement No. 48"
source_author: "Cloud Security Alliance AI Safety Initiative"
source_site: "Cloud Security Alliance"
source_date: "2026-10-23"
hn_url: "https://news.ycombinator.com/item?id=50334038"
---

**Bottom line:** China has proposed its first export controls aimed at frontier AI. Of 40 technologies added to its restricted-export catalogue, 23 are AI. The catalogue additions are a draft open for comment until November 15, 2026, but a registration requirement for exporting listed technology applies now.

## What is controlled

- **Models:** weights, checkpoints, source code, architecture and training/inference know-how are restricted if a model meets any one test: at least 10^26 training FLOPs, at least 2 trillion parameters, or a global top-10 position on public rankings or call volume at the time of export. The ranking test is dynamic and names no ranking body, so a model's status can change after deployment. The CSA authors estimate no released Chinese model currently exceeds the compute threshold.
- **Agents:** controlled separately from the model, using benchmarks. Examples: planning with 20+ step subtask chains, 60+ concurrent agents, or 200+ integrated tools. Agents built for critical infrastructure, defense or core government use are prohibited outright.
- **Offensive security:** AI malicious-code generation and adaptive evasion are prohibited; AI-assisted vulnerability discovery is restricted (for example, autonomously chaining 2+ flaws into a working exploit).
- **Chips and clusters:** AI chip design at 14 nm or below and large-cluster technologies are restricted.
- **Exclusion:** technology already public through open source or academic publication is not managed.

## What it means in practice

- API, app, web, SaaS and lightweight on-device use do not count as exporting a model. Deploying a restricted model locally outside China does.
- Transfers of ownership, licensing for overseas use, and changes of control to a foreign entity can trigger registration. Analysts say this may even capture intra-group code sharing.
- Exporters must judge for themselves whether a technology is in scope.
- The 10^26 FLOP figure mirrors the one used in the 2023 US executive order and the since-rescinded 2025 AI Diffusion rule. MOFCOM calls the revision routine and not aimed at any country.
