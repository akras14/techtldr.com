---
name: techtldr
description: Publish an article summary to techtldr.com. Use when the user runs /techtldr with an article URL, optionally with a summary they pasted or a file path holding one, or asks to post/publish a summary to techtldr — including batches like "summarize the top HN posts".
---

# /techtldr — publish an article summary

Input: an article URL, and optionally a summary the user already has (pasted text or a file path).
Output: one new file, `content/posts/<slug>.md`, committed and pushed. GitHub Actions deploys `main`.

## 1. Sync

Run `git pull --rebase origin main` first so you never push from a stale copy.

## 2. Read the source — the whole thing

Get the full text, not a model's summary of it. Prefer raw HTML → plain text over tools that summarize the page for you, since those drop the numbers and caveats a good summary needs.

- **Most sites:** `curl -sL -A '<a desktop Chrome user agent>' <url>`, then strip tags (keep headings and list items).
- **X / Twitter posts and long-form X articles:** X blocks direct fetches. Use fxtwitter:
  - post or article: `https://api.fxtwitter.com/<user>/status/<id>` — an article's text is in `tweet.article.content.blocks[].text`, its links in `entityMap`
  - replies: `https://api.fxtwitter.com/2/conversation/<id>`
- **Blocked or paywalled (403, empty body, login wall):** say so and ask the user to paste the text. Never summarize from the headline or your memory.

Pull out: title, author, publication/site name, original publish date (YYYY-MM-DD). If a field truly isn't there, omit it — never fill an author or date from memory. If you think you know the author, confirm the name appears in the page HTML first.

## 3. Write the summary

**No summary given → write one.**

- **First paragraph is the bottom line:** the conclusion and why it matters, in 2–4 sentences. It doubles as the post-list summary, so it must stand alone.
- **Length:** medium, about 400–700 words, scaled to the article. A reader should be able to skip the original for most purposes.
- **Shape follows the article:**
  - List-shaped article (N differences, N tips) → keep the N items, each opening with a **bold one-line claim** followed by 1–3 sentences.
  - Narrative or argument → short sections with plain-language headings.
  - Benchmarks, prices, specs → a table or tight bullets with the numbers.
- **Explain jargon in a few words the first time** ("compaction — summarizing older context"). Assume a smart developer outside this niche, not an insider.
- **Keep the author's confidence level.** "Points toward a new chapter" is not "is the next chapter." "Can continue while the conversation is idle" is not "outlives the conversation." Never upgrade a hedge, a "may," or a single example into a general claim.
- **Paraphrase.** Direct quotes stay short — one sentence at most, in quotation marks, and only when the exact wording matters.
- **No opinions in the main body.** The body reports what the article says. Context and critique go in "Beyond the article" (step 4).
- No preamble ("This article discusses…"), no sign-off, no title heading, no attribution line — the template renders those from front matter.

**Summary given → keep the user's wording.** Only:
- Remove chat wrapper text ("I'll pull that post and give you the BLUF.", trailing offers to help).
- Move the bottom line to the first paragraph if it isn't already there. Don't rewrite it.
- Check it against the source: flag claims the source doesn't support (including upgraded hedges) and any near-verbatim passage longer than about two sentences. Suggest fixes; don't change wording silently.

## 4. Enrich: "Beyond the article" (optional)

Add a final section when you can find something that genuinely adds to the article. Skip it when you can't — an empty or padded section is worse than none.

Good sources, in order:
1. **Primary sources the article links to** — the announcement, spec, paper, or repo it's describing. Use them to add key facts the article skipped, or to check whether its claims still hold.
2. **Substantive discussion** — X replies (fxtwitter conversation API), Hacker News comments (`https://hn.algolia.com/api/v1/items/<hn_id>`), or the comments section. Keep only replies that raise a real point: a gap, a counterexample, a correction, a production experience. Ignore praise, jokes, and spam.
3. **Answers to the discussion's open questions**, when a primary source answers them.

Rules:
- Open with an italic line saying this is added context, not part of the original.
- **Link every outside claim** to where it came from. Attribute reply points to "a reply" or "commenters" — don't present them as the author's.
- **Verify before repeating.** A reply claiming X inspired Y is a rumor until a primary source says so; leave it out or label it unconfirmed.
- Keep it to about 300 words.

## 5. Verify before showing the user

1. **Fact-check** every number, name, date, and quote in the post against the source text. Fix anything you can't find in the source.
2. **Overlap check** — find any run of 10+ consecutive words copied from the source:
   ```python
   import re
   W = lambda t: re.findall(r"[a-z0-9']+", t.lower())
   post, src = W(post_body), W(source_text)
   grams = {tuple(src[i:i+10]) for i in range(len(src) - 9)}
   hits = [" ".join(post[i:i+10]) for i in range(len(post) - 9) if tuple(post[i:i+10]) in grams]
   ```
   Short factual phrases (a model name plus a number, a formula, a list of bug classes) are fine. Reword anything that reads like the author's sentence.
3. If `hugo` is installed, run `hugo --quiet` and confirm `public/<slug>/index.html` exists.

## 6. Pick the slug

Short kebab-case from the title, 3–6 words, no stop-word clutter (e.g. `postgres-is-enough`).
It must not collide with:
- an existing file in `content/posts/`
- site paths: `search`, `archives`, `posts`, `tags`, `categories`, `page`, `index.xml`
- **an old alexkras.com URL.** techtldr.com forwards unknown paths to alexkras.com, so a post with the same slug would hijack old links. Check the sitemap:
  `curl -s https://alexkras.com/sitemap.xml | grep -q "alexkras.com/<slug>/<" && echo taken`.
  Taken → append `-tldr`. Don't probe `https://alexkras.com/<slug>/` directly: alexkras.com 301s every unknown path to its home page, so status codes can't tell real posts from missing ones. If the sitemap can't be fetched, tell the user you couldn't verify it.

## 7. Write the file

`content/posts/<slug>.md`:

```markdown
---
title: "<headline for the summary — usually the article title, sharpened>"
slug: "<slug>"
date: <current time from `date +%Y-%m-%dT%H:%M:%S%z`, never later — Hugo silently skips future-dated posts>
summary: "<the bottom-line paragraph, plain text, quotes escaped>"
source: "<article URL>"
source_title: "<original article title>"
source_author: "<author>"
source_site: "<publication name>"
source_date: "<YYYY-MM-DD>"
---

<bottom-line paragraph>

<rest of the summary>

## Beyond the article   (optional, see step 4)
```

Omit any `source_*` field you don't know rather than leaving it empty.

## 8. Review, then publish

Show the user the full file and the slug. Wait for approval or edits — never push unreviewed.

After approval:
1. `git add content/posts/<slug>.md && git commit -m "Add summary: <title>"`
2. `git push origin HEAD:main`
3. If pushing to `main` is refused (some web sessions only allow pushing a work branch), push the current branch instead and tell the user it must be merged into `main` to go live.

Reply with the live URL: `https://techtldr.com/<slug>/` (live a minute or two after the deploy finishes).

**If the user isn't around to review** (e.g. "do these overnight, I'll check in the morning"): put the posts on a branch and open a PR instead of pushing to `main`. The PR description lists each post, what was skipped and why, and what was verified.

## Batches ("summarize the top N on Hacker News")

- Front page: `https://hacker-news.firebaseio.com/v0/topstories.json`, then `/v0/item/<id>.json` for each.
- **Pick** substantive articles and essays you can read in full.
- **Skip:**
  - Show HN, tool landing pages, bare GitHub repos, Wikipedia
  - paywalls and pages that block fetching
  - thin or content-farm sources
  - stories centered on private individuals — especially minors — facing unproven allegations
- Mix topics, and say in the PR what you skipped and why.
- Give each post a distinct timestamp a few minutes apart (all in the past) so the home page order is stable.

## Removal requests

If the user asks to take a post down: `git rm content/posts/<slug>.md`, commit "Remove summary: <title> (author request)", push to `main`.
