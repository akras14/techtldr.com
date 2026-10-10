---
name: techtldr-hn
description: Unattended loop that auto-publishes techtldr.com summaries of Hacker News stories with 20+ points. Use when a scheduled routine runs /techtldr-hn.
---

# /techtldr-hn

Runs on a schedule with nobody watching. Follow the `/techtldr` skill for each post, with these changes.

1. **Sync:** `git pull --rebase origin main`.

2. **Candidates:** HN stories from the last 24 hours with 20 or more points:
   `curl -sg 'https://hn.algolia.com/api/v1/search_by_date?tags=story&hitsPerPage=200&numericFilters=points%3E%3D20,created_at_i%3E<now-86400>'`
   Drop any story whose ID is already in `hn-seen.txt` or whose URL is already a `source:` in `content/posts/`.

3. **Pick candidates, brutally.** This feed is for the owner's own reading. When in doubt, skip.
   - **In-topic only:** software engineering, programming languages, infrastructure and databases, security, AI tooling and agents, and AI business/policy news (acquisitions, lab moves, regulation).
   - **Out:** politics, economics, culture, health, gaming and entertainment news, and general science and math (unless it's about computing). Record these as `skipped off-topic`.
   - **Also skip:** Ask HN posts, job posts, stories without a URL, videos, paywalls and blocked pages, bare repo or product landing pages with little to summarize, and pieces centered on accusations against private individuals.
   - **Rank, then cut:** from the in-topic candidates you can read in full, take the highest-point stories first.

   Append every story you look at to `hn-seen.txt` as `<id> posted <slug>` or `<id> skipped <short reason>`, so later runs don't re-check it. Don't record stories you never looked at.

4. **Write the posts** with `/techtldr` steps 2–5. Publish at most 3 posts per run; leave the rest unrecorded for the next run.

5. **Publish without review:** commit the posts and `hn-seen.txt` together, then `git pull --rebase origin main` and `git push origin HEAD:main`. Don't open a PR.
