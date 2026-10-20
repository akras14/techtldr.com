---
title: "Vite+ cut one monorepo's CI from 4 minutes to 50 seconds"
slug: "vite-plus-ai-agent-workflow"
date: 2026-10-20T08:05:00+0000
summary: "A frontend engineer reports that after three weeks with Vite+ across four projects, he now puts it in the first prompt of every new Vite app he has an AI agent set up. The main win is not speed but one command set (vp create, check, test, build) that the agent, the developer and CI all share, instead of ESLint, Prettier, TypeScript and test scripts the agent had to guess at."
source: "https://janmichalek.com/blog/vite-plus-agentni-vyvoj?lang=en"
source_title: "Vite+ changed how I build applications with AI agents"
source_author: "Jan Michálek"
source_site: "janmichalek.com"
hn_url: "https://news.ycombinator.com/item?id=50162198"
---

**Bottom line:** the author says Vite+ (the `vp` CLI) gave his AI agents one clear definition of "done" per project, and he now asks for it in the first prompt of every new Vite app. His numbers are from his own projects, not a benchmark: one monorepo's CI went from 4 minutes to 50 seconds, and a pre-commit check from 11 seconds to under 2.

## Why he switched

One project had 47 dev dependencies, seven config files and a Markdown file explaining to the agent how to run everything. Local checks and CI also treated a generated file differently, so the agent and the pipeline saw different results. Vite+ puts the everyday tasks under `vp`: Vite for dev and builds, Vitest for tests, Oxlint and Oxfmt for checks, tsdown for libraries and Vite Task for tasks. It does not replace `package.json`, `tsconfig` or the package manager.

## The workflow

- Have the agent create the project with `vp create`, then verify with `vp check`, `vp test` and `vp build`.
- `vp check` covers formatting and lint. It covers types only when `typeAware` and `typeCheck` are both enabled, which `vp create` does by default. In an existing project, verify that setting.
- Constrain the agent in the prompt: one package manager, one lockfile, no second linter or formatter, and report actual versions, commands and results. Without that, one unattended agent left him with both Prettier and Oxfmt, three lockfiles and unreliable CI.
- Put the same check commands in the repo's agent instructions and in CI. Human review before merge remains his job.

## Where it got awkward

- `vp migrate` took about 4 minutes on an older monorepo and converted ESLint, Prettier and tsup with nothing breaking, but plugins and custom rules may need manual work. Treat it as the start of a migration and review the diff.
- `vp build` is not `vp run build`. In a Next.js project the agent must not assume `vp build` replaces `next build`.
- `vp upgrade` updates the global CLI, not each project's local `vite-plus` package.
- Caching is not on for `package.json` scripts unless you enable it (for example `vp run --cache build`). Tasks defined in `vite.config.ts` are cached by default.

## Who it's for

New Vite apps where an AI does the setup. He would not blindly migrate a large app that already has a working build and tests, or replace a fast, well-documented stack just for "one command".
