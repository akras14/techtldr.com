---
name: techtldr
description: Publish an article summary to techtldr.com. Use when the user runs /techtldr with an article URL, optionally with a summary they pasted or a file path holding one, or asks to post/publish a summary to techtldr.
---

# /techtldr — publish an article summary

Input: an article URL, and optionally a summary the user already has (pasted text or a file path).
Output: one new file, `content/posts/<slug>.md`, committed and pushed to `main`. GitHub Actions deploys it.

## 1. Sync

Run `git pull --rebase origin main` first so you never push from a stale copy.

## 2. Read the source

Fetch the article. Pull out: title, author, publication/site name, original publish date (YYYY-MM-DD).
If a field truly isn't there, leave it out — never invent an author or date.
If the article can't be fetched (paywall, blocked), say so and ask the user to paste the text.

## 3. The summary

**No summary given → write one.** BLUF style, medium length (roughly 300–700 words, scaled to the article). It should let a reader skip the original for most purposes.
- First paragraph is the bottom line: the conclusion and why it matters, in 1–3 sentences.
- Then whatever structure fits the article (bullets, short sections, numbers, caveats). No fixed template.
- Paraphrase. Direct quotes stay short — a sentence or two, in quotation marks. Never reproduce passages.
- Plain language, real sentences, bold lead-ins on bullets where scanning helps.
- No preamble ("This article discusses…"), no sign-off.

**Summary given → keep the user's wording.** Only:
- Remove chat wrapper text ("Sure! Here's a summary:", trailing offers to help).
- Move the bottom line to the first paragraph if it isn't already there. Don't rewrite it.
- Compare against the fetched article. If any passage is near-verbatim and longer than about two sentences, point it out to the user and suggest a paraphrase. Don't change it silently.

Don't put a title heading or attribution line in the body — the template renders both from front matter.

## 4. Pick the slug

Short kebab-case from the title, 3–6 words, no stop-word clutter (e.g. `postgres-is-enough`).
It must not collide with:
- an existing file in `content/posts/`
- site paths: `search`, `archives`, `posts`, `tags`, `categories`, `page`, `index.xml`
- **an old alexkras.com URL.** techtldr.com forwards unknown paths to alexkras.com, so a post with the same slug would hijack old links. Check with
  `curl -s -o /dev/null -w '%{http_code}' https://alexkras.com/<slug>/`.
  Anything other than `404` means taken → append `-tldr`. If the check can't run (no network), tell the user you couldn't verify it.

## 5. Write the file

`content/posts/<slug>.md`:

```markdown
---
title: "<headline for the summary — usually the article title>"
slug: "<slug>"
date: <now, ISO 8601 with timezone>
summary: "<the bottom-line paragraph, plain text, quotes escaped>"
source: "<article URL>"
source_title: "<original article title>"
source_author: "<author>"
source_site: "<publication name>"
source_date: "<YYYY-MM-DD>"
---

<bottom-line paragraph>

<rest of the summary>
```

Omit any `source_*` field you don't know rather than leaving it empty.

## 6. Review, then publish

Show the user the full file and the slug. Wait for approval or edits — never push unreviewed.

After approval:
1. If `hugo` is installed, run `hugo --quiet` and fix any build error before committing.
2. `git add content/posts/<slug>.md && git commit -m "Add summary: <title>"`
3. `git push origin HEAD:main`
4. If pushing to `main` is refused (some web sessions only allow pushing a work branch), push the current branch instead and tell the user it must be merged into `main` to go live.

Reply with the live URL: `https://techtldr.com/<slug>/` (it's live a minute or two after the deploy finishes).

## Removal requests

If the user asks to take a post down: `git rm content/posts/<slug>.md`, commit "Remove summary: <title> (author request)", push to `main`.
