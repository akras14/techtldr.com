---
title: "Owning agent-written code means knowing enough to dive when it breaks"
slug: "here-be-monsters-agents-engineers"
date: 2026-10-27T15:04:08+0000
summary: "Adrian Quiroga argues that as coding agents write most of the code, owning a system depends less on having written it and more on knowing just enough to spot when something is wrong, and being able to dive deep on demand. He frames the Optimist and Skeptic camps with a divebot metaphor and asks how the next generation of engineers will get that judgment."
source: "https://blog.adrq.dev/here-be-monsters-what-do-you-still-need-to-know-when-agents-do-the-work"
source_title: "Here Be Monsters: What Do You Still Need to Know When Agents Do the Work?"
source_author: "Adrian Quiroga"
source_site: "Hashnode Engineering Notes"
source_date: "2026-10-27"
hn_url: "https://news.ycombinator.com/item?id=50258868"
---

**Bottom line:** The author's claim is that ownership of a software system will matter less as "having written it" and more as knowing just enough to recognize when something is wrong, plus being willing and able to dive as deep as needed. Fundamentals, breadth and just-in-time depth are what stay valuable.

## The anecdote
CI failed because SonarQube flagged `App.tsx` for cyclomatic complexity: about 30 lines, half of them whole components that Codex had written on single lines. After formatting and inspecting, the author found the code correct and warranted by the requirements. Telling Codex to satisfy SonarQube got it split into 10 files and CI passed. It went well, but it was code the author had never seen, and it easily could have gone worse.

## The framing
- **Divebots:** Agents are like bots that dive and build mining operations on their own, surfacing only with updates and "gold nuggets."
- **Optimists vs Skeptics:** Optimists relax on the boat; Skeptics warn of lost bots and shiny-but-worthless output, with carefully designed "cathedrals" turning into haphazard mansions with hidden horrors. The more detached you are, the more spectacular the failures.
- **The open problem:** Experienced practitioners can now run fleets of agents at scale, but how do you train the next generation? The extremes answer differently, from "read every line and be able to write it by hand" to the opposite.

The rest of the essay (sections on fundamentals, breadth and just-in-time depth, and "owners of the future") develops the middle path; this summary is based on the opening and framing sections.
