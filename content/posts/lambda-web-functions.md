---
title: "Lambda Web Functions run unmodified Node HTTP servers, billed on CPU time"
slug: "lambda-web-functions"
date: 2026-10-13T12:03:15+0000
summary: "AWS added a third Lambda form factor, Web Functions, that runs an ordinary port-listening HTTP server (Express, Hono, Fastify, Next.js) with no event-handler rewrite or adapter. Each environment serves up to 128 concurrent requests, and billing meters active CPU time rather than wall-clock time, which should cut costs for I/O-heavy workloads like SSR, API backends and streaming AI endpoints."
source: "https://aws.amazon.com/blogs/aws/announcing-aws-lambda-web-functions-for-fast-consistent-web-app-performance-anywhere/"
source_title: "Announcing AWS Lambda Web Functions"
source_author: "Micah Walter"
source_site: "AWS News Blog"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50069318"
---

**Bottom line:** AWS Lambda now has Web Functions, which let you deploy a standard HTTP server as-is. Your code listens on the host and port given in `AWS_LAMBDA_HTTP_ENDPOINT`, and Lambda handles scaling and routing. Billing counts CPU time actually used, so time spent waiting on a database or API call is cheap.

- **Three form factors now:** Event Functions (the classic handler model, renamed), Web Functions (HTTP-native, multi-concurrent, active-CPU billing), and Lambda MicroVMs (isolated, stateful sandboxes for untrusted or user-generated code).
- **Concurrency:** up to 128 concurrent requests per execution environment, so bursts share warm environments instead of each starting a new one. Memory is billed for as long as the environment exists.
- **Deploy:** `aws lambda-web deploy --name my-web-app --code ./my-app` creates the function, a revision and an HTTPS endpoint in under a minute. Adding `--regions` replicates to several regions behind one global URL with best-effort latency-based routing.
- **Rollouts:** revisions are immutable, and endpoints can split traffic by weight (for example 90/10) for canaries. CloudFront can front an endpoint, with origin access control restricting invocation to your distribution.
- **Limits at launch:** Node.js 24 on Arm64 only, VPC connectivity supported, available in us-east-1, us-east-2, us-west-2, ap-northeast-1 and eu-west-1.
