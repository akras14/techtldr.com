---
title: "Gheist plants secret-dumping workflows on 13,500 branches via stolen GitHub tokens"
slug: "gheist-github-token-secret-dumping"
date: 2026-10-25T22:02:33+0000
summary: "Wiz Research has tracked Gheist since September 15: an automated campaign that uses stolen GitHub tokens to push a `sync-check` workflow to every repository the token can reach, printing the repo's secrets into the workflow log. Wiz recovered 13,500+ planted branches from 850+ compromised accounts. GitHub's approval hold blocks it in public repos, but private repos and GitHub Enterprise Server are still being exploited."
source: "https://www.wiz.io/blog/gheist-attempted-scaled-abuse-of-stolen-github-tokens-stumbles"
source_title: "Gheist: Attempted Scaled Abuse of Stolen GitHub Tokens Stumbles"
source_author: "Rami McCarthy, Hila Ramati"
source_site: "Wiz Blog"
source_date: "2026-10-25"
hn_url: "https://news.ycombinator.com/item?id=50235010"
---

Wiz Research has been tracking **Gheist** since September 15, 2026: an automated campaign that takes stolen GitHub tokens and commits a workflow named `sync-check` to a new `ci-sync-xxxxxx` branch in every repository each token can access. The push triggers the workflow, which dumps the repo's secrets into its own run log. Wiz recovered more than 13,500 planted branches from over 850 compromised accounts, and the attack is ongoing.

## How it works

- The workflow prints `env` and `toJSON(secrets)` piped through `base32`. GitHub only masks secrets that appear verbatim, so the encoded values show up in full. Environment-scoped secrets, often production credentials, are included via a job matrix.
- The tool downloads the log through the GitHub API, then deletes the run seconds later. Since secrets leave through GitHub's own API, there is no C2 domain or outbound connection to block.
- One variant added a `sync-host` job for self-hosted runners that sweeps the host for 31 token formats (including AI provider keys), cloud metadata credentials and the Kubernetes token, and reaches the host root via the Docker socket. Wiz says that job is buggy.
- In enterprise organizations, the tool cycled through several users' tokens within minutes, cloned private repos first, and hours later cloud credentials from the same org were used to enumerate AWS Secrets Manager.

## What's stopping it

Since July 28, GitHub holds potentially malicious workflow runs in public repositories until a collaborator approves them. Every public Gheist run Wiz checked is stuck at `action_required`. That hold doesn't apply to private repositories or GitHub Enterprise Server, where audit logs and CloudTrail show runs completing and assuming AWS roles. About two thirds of implicated accounts are now gone from GitHub, likely due to response activity.

The same accounts also appear in other campaigns (cracked-software lure repos, free-compute mining, the DPRK-linked PolinRider malware), and the tokens trace back to infostealer logs and earlier supply-chain compromises such as Trivy in March.

## What to do

- **Hunt:** branches matching `^ci-sync-[0-9a-z]{6}$` with a single new `.github/workflows/sync-check.yml`, commit message `ci: sync config`, the `gheist` user agent, or OIDC role assumptions whose subject includes a `ci-sync-` branch.
- **Respond:** rotate every secret reachable by an affected repo (deleting the run or branch doesn't undo exposure), revoke the tokens, check owners' devices for infostealers, and treat any self-hosted runner that ran the branch as compromised.
- **Harden:** require reviewers on environments with production secrets, scope OIDC trust to protected branches or environments, prefer GitHub Apps or short-lived fine-grained tokens over PATs, and run ephemeral self-hosted runners without Docker socket access.
