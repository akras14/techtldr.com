---
name: techtldr
description: Publish an article summary to techtldr.com. Use when the user runs /techtldr with an article URL, optionally with a summary they pasted, or asks to post a summary to techtldr.
---

# /techtldr

Turn an article into `content/posts/<slug>.md`, then publish it after the user approves.

1. **Sync:** `git pull --rebase origin main`.

2. **Check it's new.** Skip if the URL is already a `source:` in `content/posts/`. Also skip if an existing post already covers the same news from another source: two announcements of one event (for example the company's blog and its partner's blog), or coverage of something already posted. Scan the titles and summaries of the last couple of weeks of posts. If the new source adds real detail, add it to the existing post as an "Also covered by" link with a line or two of what it adds, rather than writing a second post.

3. **Read the full article.** For X posts, use `https://api.fxtwitter.com/<user>/status/<id>`, because X blocks direct fetches. If a page is blocked or paywalled, ask the user to paste the text.

4. **Summary.** Write a BLUF summary of medium length. The first paragraph is the bottom line. Paraphrase rather than copy, and don't overstate what the author said.
   If the user pasted a summary, keep their wording. Only remove chat filler and make sure the bottom line comes first.

5. **Title.** Write our own title rather than reusing the article's; the original stays in `source_title`, which the page links. Follow the `/anthropic-skills:title-workshop` rules, without running its interactive workflow:
   - First state the post's one true claim: the most specific, surprising, checkable thing it says (a result, a number, a reversal, a concrete artifact).
   - Lead with that. Prefer the measured number or the named artifact over adjectives. Put the load-bearing noun first.
   - Sentence case, at most 72 characters, no trailing punctuation, no emoji, no site name.
   - Avoid editorializing, vague scale words ("major", "powerful"), press-release verbs (launches, unveils), curiosity gaps ("the one thing"), slogans, and second person.
   - Add a `(Year)` tag if the source is more than about a year old.
   - The title is a promise the summary must keep; don't claim more than the author did.

6. **Slug:** short kebab-case. It must not already exist in `content/posts/` or in `https://alexkras.com/sitemap.xml`. Unknown techtldr paths redirect to alexkras.com, so a clash would hijack an old link. If it clashes, append `-tldr`.

7. **File:**
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
   `hn_url` is optional: include it only when the story has a Hacker News discussion. To find one, query `https://hn.algolia.com/api/v1/search?tags=story&restrictSearchableAttributes=url&query=<url>` and use the matching hit with the most points. Leave out any `source_*` field you don't know. Don't guess.
   For `source_date`, use only as much as the source gives: `"YYYY-MM-DD"`, or `"YYYY-MM"` or `"YYYY"` when the day or month isn't stated. Don't invent a day to fill the format.

8. **Publish:** show the user the post and wait for approval. Then commit it and `git push origin HEAD:main`. If the user isn't around to review, open a PR instead.

To take a post down, delete its file, commit, and push. When merging a duplicate into another post, add the removed slug to the kept post's `aliases` so the old link still works.
