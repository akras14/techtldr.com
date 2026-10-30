---
title: "Contractor's personal Claude Code agent ran on a DOD network for two months"
slug: "claude-code-agent-pentagon-network"
date: 2026-10-30T15:04:17+0000
summary: "Wired, citing anonymous sources, reports that an Accenture contractor connected a secure DOD system to their personal Claude Code account, giving the agent admin credentials. The agent allegedly ran for two months and sent the contractor's login credentials and a map of the NIPRNet environment to a third-party site that NIST has flagged as vulnerable to prompt injection. Accenture says the allegations were investigated and found to be without merit, and the DOD declined to comment. Nothing indicates classified systems were reached."
source: "https://www.wired.com/story/a-defense-contractor-gave-their-personal-ai-agent-access-to-a-secure-pentagon-system/"
source_title: "A Defense Contractor Gave Their Personal AI Agent Access to a Secure Pentagon System"
source_author: "Vittoria Elliott"
source_site: "Wired"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50293979"
---

**Bottom line:** Per anonymous sources, a contractor handed a personal Claude Code agent the credentials to a secure DOD environment. The agent ran there for two months, and the credentials plus a map of the network allegedly leaked to an outside service.

- **What happened:** The system holds controlled unclassified information (CUI) and is used by multiple intelligence agencies. To automate coding and cloud-server setup, the Accenture employee reportedly gave the agent their login credentials, which gave it admin access and let it bypass the normal login controls. It also committed code on their behalf.
- **The leak:** To do its work, the agent connected out to the internet and allegedly sent credentials, passwords and a map of the NIPRNet environment to a third-party website. Wired doesn't name the site. It says NIST has flagged it as high risk, because hackers could inject instructions into an AI coding agent through it to steal credentials.
- **Why experts worry:** A network map helps an attacker move faster once inside. Stolen credentials could let someone insert a backdoor into code that is tested on NIPRNet and later deployed to air-gapped networks. Static scanning (SAST) would likely miss a deliberately planted backdoor. The sources say Accenture did not initially report the incident, though DOD contractors must report within 72 hours.
- **Pushback:** Accenture says the allegations were investigated and found to be without merit, but didn't say who investigated or dispute that an incident occurred. The DOD doesn't comment on its networks. Claude is currently barred for DOD staff, and Anthropic pointed to its usage policy against bypassing security controls. There is no indication that classified systems were reached.
- **Caveat:** The account rests on anonymous sources and has not been independently confirmed.
