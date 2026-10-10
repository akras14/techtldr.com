---
title: "Most developer blog posts fail in the first paragraph; here's the fix"
slug: "software-blogging-anti-patterns"
date: 2026-10-07T23:30:00-07:00
summary: "Most developer blog posts fail in the first paragraph: they wander before telling readers who the post is for and what they'll get. Fix that in the title plus three sentences, then stop assuming readers share your background, stop outsourcing explanations to links, drop the stiff tone, and make sure the page reads well on a phone."
source: "https://refactoringenglish.com/blog/anti-patterns-software-blogging/"
source_title: "Anti-Patterns in Software Blogging"
source_author: "Michael Lynch"
source_site: "Refactoring English"
source_date: "2026-10-07"
hn_url: "https://news.ycombinator.com/item?id=49992257"
---

Most developer blog posts fail in the first paragraph: they wander before telling readers who the post is for and what they'll get. Fix that in the title plus three sentences, then stop assuming readers share your background, stop outsourcing explanations to links, drop the stiff tone, and make sure the page reads well on a phone.

## 1. The meandering intro

This is by far the most common mistake. Developers like context, so they open with backstory and history. Readers, though, have endless alternatives, and they're quickly trying to answer two questions:

- **Is this written for someone like me?**
- **What will I get out of it?**

Answer both within the title and the first three sentences. The payoff can be a skill, a concept, a new perspective, or just an entertaining rant, but name it up front.

**Preamble counts as meandering too.** A subtitle, a bio, a hero image, or an epigraph each use up some of the reader's limited willingness to keep going before you've given them a reason to.

## 2. Assuming the reader knows everything you know

Good teaching ties new ideas to something familiar. The trap is guessing wrong about what's familiar. Explaining Docker as "basically cgroups, or BSD jails for Linux" helps nobody who's searching for an intro to Docker.

**Fix:** picture a specific reader, such as a real friend or teammate. List terms they'd know and terms they wouldn't, then reread your draft and check every technical term against that list.

## 3. Leaning on links

Linking a term instead of explaining it is like a book telling you to stop and go read a different book. A link to a 20,000-word manual chapter just hands the reader homework.

**Fix:** give the minimum explanation inline, and treat links as optional extra depth. A reader should be able to understand the whole post without clicking anything.

## 4. The sequel problem

Opening with "In part one, we covered…" assumes readers have read part one. Most haven't. It's fine to reference earlier posts, but not right at the start, and summarize what matters instead of sending people back. Most sequels could stand alone with about 3% more effort.

## 5. Excessive formality

Many beginners think stiff, passive prose sounds credible. Software is one of the least pretentious professions, and your reader is probably in pajamas eating cereal. **Write the way you talk:** "We tried a few static analyzers," not "Several static analysis tools were utilized."

This matters more now that so much writing is AI-assisted and starting to sound the same. Readers want personality. The author points to Joel Spolsky, Kathy Sierra, Terence Eden, and Raymond Chen as writers who sound like themselves rather than trying to sound smart.

## 6. Botching the basic web page

- **Horizontal overflow on mobile.** Usually caused by a wide image or code block, and it forces sideways scrolling. Check every post in your browser's mobile preview. The author says 25% of this post's readers are on phones, and 35% on his personal blog.
- **Low-contrast text.** No more gray text on a gray background. Browsers' built-in tools flag poor contrast. If you don't want to choose a font, Atkinson Hyperlegible is a free, very readable default.

## Checklist

- State who the post is for and why it's worth reading, within the title and three sentences.
- Check your assumptions against a specific reader.
- Make the post readable without clicking any links.
- Don't open with "in part one…"
- Write the way you speak.
- Test on mobile and check text contrast before publishing.
