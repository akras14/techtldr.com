---
title: "Paying 6 volunteers €25 each to follow a README out loud exposed what the author missed"
slug: "paid-readme-testers"
date: 2026-10-11T13:02:23+0000
summary: "Terence Eden paid about €150 total (€25 per hour) to have volunteers screen-share and think aloud while following the ActivityBot README for the first time. The sessions surfaced errors and assumptions he could not see himself, such as a wrong demo link, confusing section order, and unexplained terms. He fixed the README after each session and retested with the next person, and recommends watching real people do this over simulating users with an LLM."
source: "https://shkspr.mobi/blog/2026/10/i-paid-people-to-try-and-follow-my-readme/"
source_title: "I paid people to try and follow my README"
source_author: "Terence Eden"
source_site: "shkspr.mobi"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50042219"
---
**Bottom line:** Having real people follow your README while talking aloud finds problems you cannot see yourself. The author paid around €150 in total (€25 per hour per volunteer) and his README became demonstrably easier to follow.

**Method**
- Part of an NLnet grant for ActivityBot, which promised to test the install experience with real users. Volunteers were recruited on Mastodon.
- Each volunteer shared their screen on a video call and narrated what they did, what confused them, and what frustrated or pleased them. The author took handwritten notes.
- After each session he updated the README, then retested with the next person.

**What turned up**
- The link to the demo tool was wrong.
- Some people read the README in a terminal.
- Unclear steps such as renaming a file, or a hidden file.
- Confusing quoting rules, section ordering, and jokes.
- Terminology needing explanation, and the README never said what the software does.
- A section interesting to the author was confusing to everyone else.
- Web servers differ in how they handle permissions.

**Why not an LLM?** He wants to talk to real people, who bring unique perspectives and whose frustration in their voice signals a mistake needs fixing. He does not claim the README is perfect, and says you can skip payment if you find people willing to do it free.
