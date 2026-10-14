---
title: "GitHub Actions gets actions.lock to pin workflow dependencies to commits"
slug: "github-actions-lockfiles-preview"
date: 2026-10-14T23:02:47+0000
summary: "GitHub Actions workflow lockfiles are in public preview. A generated .github/workflows/actions.lock records the exact commits behind every action and reusable workflow a workflow uses, so a moved tag or branch can't silently change what runs."
source: "https://github.blog/changelog/2026-10-14-github-actions-workflow-lockfiles-in-public-preview/"
source_title: "GitHub Actions workflow lockfiles in public preview"
source_site: "GitHub Changelog"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50088044"
---

GitHub Actions workflow lockfiles are now in public preview (Enterprise Cloud, Team and Free plans). They address a supply-chain gap: tags and branches can move after you review a workflow, so a compromised action can change what your workflow executes without any change in your repository.

- **Pinning:** the `gh actions-lock` CLI extension generates `.github/workflows/actions.lock` with reviewable commits for actions, their transitive dependencies, and reusable workflows.
- **Gradual adoption:** workflows are onboarded one at a time. Once onboarded, every dependency must have a lockfile entry, and Actions runs the recorded commit even if the tag or branch has moved.
- **Staleness check:** the extension can verify the lockfile is current without modifying files, including as a CI check.
- **Policy:** a "Require lockfile" setting lets enterprise, org and repo admins force workflows to use lockfiles.
- **Dependabot:** version-update PRs for onboarded workflows also regenerate the matching lockfile entry.

GitHub notes that locking doesn't judge whether a dependency is safe, so review before you lock.
