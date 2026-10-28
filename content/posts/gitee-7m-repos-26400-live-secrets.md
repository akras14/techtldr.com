---
title: "26,400 live credentials found in 7.25M Gitee repos, at $3,100 in compute"
slug: "gitee-7m-repos-26400-live-secrets"
date: 2026-10-28T18:02:39+0000
summary: "Independent researcher Ben Zimmermann, with MongoDB's Kingfisher team, scanned 7,256,531 public repositories on Gitee, China's national code-hosting platform, with TruffleHog and Kingfisher and found 26,400 unique credentials that verified as live. That is 3.64 per 1,000 repos, higher than published scans of GitLab (3.11) and Bitbucket (2.39). The most common leak was Alibaba Cloud access keys, about 1 in 766 repos. The whole run took about a month and cost $3,097.64 on Modal."
source: "https://www.mongodb.com/company/blog/technical/scanning-7-point-25-million-public-repositories-chinas-largest-git-platform-for-secrets"
source_title: "Scanning 7.25 Million Public Repositories on China's Largest Git Platform for Secrets"
source_author: "Ben Zimmermann"
source_site: "MongoDB"
source_date: "2026-10-28"
hn_url: "https://news.ycombinator.com/item?id=50275375"
---
A guest post on MongoDB's blog reports a scan of 7,256,531 public Gitee repositories using both TruffleHog and Kingfisher, which only report credentials that authenticate against the vendor's API. The result was 26,400 unique live credentials, or 3.64 per 1,000 repos. The post compares that with earlier TruffleHog-only scans: GitLab at 3.11 and Bitbucket at 2.39. Because TruffleHog alone covers fewer Chinese vendors, its Gitee-only figure is 1.76 per 1,000, so the comparison depends on which scanner you count. Findings included an active organization-admin credential for a major enterprise AI platform and thousands of cloud access keys.

## Details

- **Enumeration was the hard part.** Gitee has no paginated project listing and no working code search. The author seeded 1.7M usernames from a user-search endpoint with trigram prefixes and the Explore feed, scraped per-user project and starred pages, and queried the internal Elasticsearch behind the site search. That search caps at 200 results per query, which he got around by splitting queries by language and license buckets.
- **Scanning:** one Modal function per repo bare-cloned over SSH and ran both scanners on the clone, at 350 concurrent containers in 50 batches of about 150K repos. It took roughly a month, mostly slow cross-Pacific clones, and cost $3,097.64, covered by a Modal credit grant.
- **The scanners complement each other.** TruffleHog (12,737) and Kingfisher (14,664) overlapped on only 1,001. TruffleHog found far more Alibaba Cloud keys (9,428 vs. 993). Kingfisher found far more DeepSeek (2,940 vs. 230), OpenAI, HuggingFace and Zhipu credentials.
- **Mix:** Alibaba Cloud AccessKey pairs were the most common, about 1 in 766 repos. Roughly half the credentials (12,242) were committed in 2025. Committer emails were mostly qq.com, 163.com and gmail.com, so attribution to companies is only a floor, though Huawei, ByteDance, Google, Microsoft and Oracle domains appear.
- **Disclosure:** the author emailed the vendors with the most exposed credentials but did not notify individual committers. Revocation status was not confirmed at publication. Many Chinese vendors lack bug bounty or security contact channels.
