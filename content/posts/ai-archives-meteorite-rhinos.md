---
title: "Pointing AI at 400 Years of Archives Found a Meteorite, Lost Rhinos and Unrecorded Eruptions"
slug: "ai-archives-meteorite-rhinos"
date: 2026-10-10T00:03:26+0000
summary: "Software engineer Jesse Waites built an AI pipeline to search millions of digitized Dutch East India Company pages and old newspapers. He reports candidate finds: an unrecorded 1812 meteorite fall in India, three captive Javan rhinos, and three eruptions missing from the Smithsonian list. They are not yet confirmed by specialists."
source: "https://jessewaites.com/blog/post/i-pointed-ai-at-400-years-of-archives/"
source_title: "I Pointed AI at 400 Years of Historical Archives. It Found a Forgotten Meteorite, Lost Rhinos, and Unrecorded Volcanic Eruptions."
source_author: "Jesse Waites"
source_site: "jessewaites.com"
source_date: "2026-10-08"
---

**Bottom line:** Inspired by historian Benjamin Breen's AI-assisted discovery of a new dodo eyewitness account, Waites built a pipeline to mine huge digitized archives. He reports several candidate discoveries, checked against original page scans and the catalogues he could access, but not yet reviewed by specialists. He says he has contacted them.

**Method.**
- Sources included 4.35 million pages of Dutch East India Company records (GLOBALISE transcriptions), Dutch and American newspapers, and ship logs.
- Passages were embedded so a search matches meaning rather than spelling. This matters because old Dutch spells "rhinoceros" about fifteen ways.
- A tiny, cheap model made the first pass with narrow yes/no questions. Reading 59,000 elephant mentions cost about $3. A larger model then read the survivors, and an agent checked each against the scanned original.
- Before trusting empty searches, he checked the pipeline could find known events such as the dodo, Laki and Tambora.

**Candidate finds:**
- **Meteorite.** An 1812 letter reprinted in a Batavia newspaper describes a stone falling near Pandharpur in Maharashtra on 6 August 1812. It isn't in the six catalogues he checked, and would predate the current earliest Maharashtra record by 26 years.
- **Rhinos.** Letters from 1738–1740 describe three live Javan rhinos meant as gifts for the King of Kandy, and none arrived alive. He found no record of these shipments in the catalogue he checked.
- **Eruptions.** Of 769 eruption reports, three survived checking as missing from the Smithsonian list: Gamkonora (1722), Ciremai (1712) and Slamet (1780).

**What failed.** He found no sign of the mysterious 1808/09 eruption, despite searching many sources.

**Caveats.** He stresses this was AI-assisted, with him steering. He also lists newspaper oddities, such as phantom crowds and a sea serpent, which he explicitly did not verify. He is open-sourcing the toolkit, Antiquity.
