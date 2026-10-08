# techtldr.com

AI summaries of articles, BLUF style. Hugo + PaperMod, deployed to GitHub Pages by `.github/workflows/deploy.yml`.

- **Add a post:** run `/techtldr <url>` in Claude Code (locally in this folder or on claude.ai/code). Paste a summary after the URL to publish one written elsewhere.
- **Preview locally:** `hugo server`
- **Posts** live in `content/posts/` and publish at `techtldr.com/<slug>/`.
- **Unknown paths** forward to the same path on alexkras.com (`layouts/404.html`), so old techtldr links keep working.
