---
title: "Automating 95% of incidents leaves SREs unready for the other 5%"
slug: "ai-sre-deskilling-ironies-of-automation"
date: 2026-10-30T17:02:40+0000
summary: "SigNoz argues the real risk of AI in incident response isn't replacement but deskilling: as AI handles routine triage and remediation, SREs lose the hands-on practice needed for the rare novel outage that still lands on them."
source: "https://newsletter.signoz.io/p/ai-isnt-replacing-sres-its-deskilling"
source_title: "AI Isn't Replacing SREs. It's Deskilling Them."
source_author: "Elizabeth"
source_site: "SigNoz Observability Real Talk"
source_date: "2026-02-28"
hn_url: "https://news.ycombinator.com/item?id=50305693"
---

**Bottom line:** The author's thesis is that AI is unlikely to replace SREs but may quietly make them worse at the work only humans are left to do. As AI absorbs routine triage and known-issue fixes, humans see mostly novel, high-stakes incidents while getting less practice at the fundamentals.

**The framework.** The piece leans on Lisanne Bainbridge's 1983 "Ironies of Automation": the more a process is automated, the more critical the operator is when automation fails, and the less practice they have. The author maps this onto SRE tiers (as framed by PagerDuty): known issues fully automated, partially understood ones get AI suggestions with human validation, and only novel cascading failures stay human-led. If humans only see tier 3, where does their intuition come from?

**Evidence from other fields.** The post cites a study where endoscopists' unassisted polyp detection dropped from 28% to 22% after a period of AI assistance, and pilots whose manual flying and situational awareness degrade with heavy autopilot use. It adds that AI makes work feel easier, so people misjudge their own skill, and green dashboards hide the atrophy.

**In SRE practice.** Engineers stop reading raw logs because AI summarizes them, stop forming their own hypotheses because AI proposes root causes, and stop building mental models of architecture because AI maps dependencies. The author also admits to reaching for AI first and feeling the "rust."

This is an opinion piece drawing on analogy and the author's experience rather than SRE-specific measurements.
