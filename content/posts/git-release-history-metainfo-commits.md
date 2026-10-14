---
title: "One Git ref per app instead of thousands of release tags"
slug: "git-release-history-metainfo-commits"
date: 2026-10-14T01:02:27+0000
summary: "Mark Dominus describes a 2019 proposal for recording deployment history in a monorepo without creating a tag ref per deploy. Keep one ref per (tier, app) that points to a chain of two-parent \"metainfo\" commits: the first parent is the previous release record, the second is the commit actually released. Then git log on that ref is the release history, with no thousands of refs slowing packed-refs lookups. He doesn't think it was adopted and notes it may have unforeseen drawbacks."
source: "https://blog.plover.com/prog/git/history-proposal.html"
source_title: "Maybe a better way to track the release history of a Git repo"
source_author: "Mark Dominus"
source_site: "blog.plover.com"
source_date: "2026-10-06"
hn_url: "https://news.ycombinator.com/item?id=50067721"
---

At ZipRecruiter, every deploy to staging or production created a tag ref like `refs/release/stg.www.our-app-<timestamp>-treesha-<sha>`. After thousands of these, ref resolution (a scan of `packed-refs`) got slow for everyone, and `git fetch` and `git branch` output filled with release tags. Tags are also mutable, and the metadata lived only in the tag name, so even simple questions ("when did we last deploy to prod?", "what was the release before this one?") meant listing, filtering and sorting all tags.

The proposal keeps a single ref per (tier, app), such as `stg.www.our-app`, pointing at a chain of metainfo commits:

- **Structure:** each metainfo commit has two parents. The first is the previous metainfo commit; the second is the real release commit. It's created with `git commit-tree -p <current-ref> -p H H^{tree}` and pushed to that one ref.
- **History for free:** fetch the ref into a local name (`HISTORY`) and `git log HISTORY` lists every release. `HISTORY^2` is the current release commit, `HISTORY~n^2` the release from n deploys ago, and `git diff HISTORY~1^2 HISTORY^2` compares the last two releases.
- **Metadata:** the commit message, trailers or the commit's tree can hold the tier, app, tree ID and anything else; the timestamp is the commit's own.
- **Rollback:** a rollback can point at the exact same release commit as before.

Dominus says he doesn't think the proposal was ever adopted, and it may have drawbacks he hadn't foreseen.
