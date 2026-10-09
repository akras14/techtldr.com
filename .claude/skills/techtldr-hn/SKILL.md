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

3. **Pick good candidates:** an article worth a summary that you can read in full. Skip:
   - Ask HN posts, job posts, and stories without a URL
   - videos, paywalls, and blocked pages
   - bare repo or product landing pages with little to summarize
   - pieces centered on accusations against private individuals

   Append every story you look at to `hn-seen.txt` as `<id> posted <slug>` or `<id> skipped <short reason>`, so later runs don't re-check it.

4. **Write the posts** with `/techtldr` steps 2–5. Publish at most 5 posts per run; leave the rest unrecorded for the next run.

5. **Publish without review:** commit the posts and `hn-seen.txt` together, then `git pull --rebase origin main` and `git push origin HEAD:main`. Don't open a PR.
