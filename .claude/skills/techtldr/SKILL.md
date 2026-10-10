---
name: techtldr
description: Publish an article summary to techtldr.com. Use when the user runs /techtldr with an article URL, optionally with a summary they pasted, or asks to post a summary to techtldr.
---

# /techtldr

Turn an article into `content/posts/<slug>.md`, then publish it after the user approves.

1. **Sync:** `git pull --rebase origin main`.

2. **Read the full article.** For X posts, use `https://api.fxtwitter.com/<user>/status/<id>`, because X blocks direct fetches. If a page is blocked or paywalled, ask the user to paste the text.

3. **Summary.** Write a BLUF summary of medium length. The first paragraph is the bottom line. Paraphrase rather than copy, and don't overstate what the author said.
   If the user pasted a summary, keep their wording. Only remove chat filler and make sure the bottom line comes first.

4. **Slug:** short kebab-case. It must not already exist in `content/posts/` or in `https://alexkras.com/sitemap.xml`. Unknown techtldr paths redirect to alexkras.com, so a clash would hijack an old link. If it clashes, append `-tldr`.

5. **File:**
   ```markdown
   ---
   title: "..."
   slug: "..."
   date: <output of `date +%Y-%m-%dT%H:%M:%S%z`; future dates don't publish>
   summary: "<the bottom-line paragraph>"
   source: "<url>"
   source_title: "..."
   source_author: "..."
   source_site: "..."
   source_date: "YYYY-MM-DD"
   hn_url: "https://news.ycombinator.com/item?id=<id>"
   ---
   ```
   `hn_url` is optional: include it only when the story has a Hacker News discussion. Leave out any `source_*` field you don't know. Don't guess.
   For `source_date`, use only as much as the source gives: `"YYYY-MM-DD"`, or `"YYYY-MM"` or `"YYYY"` when the day or month isn't stated. Don't invent a day to fill the format.

6. **Publish:** show the user the post and wait for approval. Then commit it and `git push origin HEAD:main`. If the user isn't around to review, open a PR instead.

To take a post down, delete its file, commit, and push.
