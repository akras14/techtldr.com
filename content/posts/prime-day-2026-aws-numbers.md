---
title: "Prime Day 2026 by the Numbers: What It Took to Run on AWS"
slug: "prime-day-2026-aws-numbers"
date: 2026-10-07T23:45:00-07:00
summary: "AWS's annual Prime Day stats post shows the four-day sale running at enormous scale: DynamoDB peaked at 192M requests per second, Lambda handled over 2.3T invocations a day, and Graviton chips ran up to 49% of Amazon.com's EC2 compute. The most interesting number: Amazon ran 44,000+ fault-injection experiments beforehand, over six times as many as the year before."
source: "https://aws.amazon.com/blogs/aws/all-the-numbers-amazon-prime-day-2026-powered-by-aws/"
source_title: "All the numbers: Amazon Prime Day 2026 powered by AWS"
source_author: "Channy Yun"
source_site: "AWS News Blog"
source_date: "2026-10-06"
---

AWS's annual Prime Day stats post shows the four-day sale running at enormous scale: DynamoDB peaked at 192M requests per second, Lambda handled over 2.3T invocations a day, and Graviton chips ran up to 49% of Amazon.com's EC2 compute. The most interesting number: Amazon ran 44,000+ fault-injection experiments beforehand, over six times as many as the year before.

Prime Day 2026 ran June 23–26. This is the tenth year AWS has published a numbers post like this.

## The standout figures

**Compute**
- **Graviton** (AWS's own ARM chips) ran up to **49%** of the EC2 compute Amazon.com used.
- **Lambda:** over **2.3 trillion** invocations per day.
- **Fargate:** an average of **158M container tasks per day** through ECS, up about **48%** from last year.

**Data**
- **DynamoDB:** over **59 trillion** requests across the event, peaking at **192M requests/second**, with single-digit-millisecond latency.
- **ElastiCache:** peaked above **2.3 quadrillion requests per day**.
- **EBS:** over 24.8 trillion I/O operations, moving more than an **exabyte a day**.
- **Aurora:** hundreds of billions of transactions, about 5.5 PB stored.

**Messaging and streaming**
- **SQS:** peak of **213M messages/second**.
- **SNS:** 5 trillion messages in a single day.
- **Kinesis:** peak of 988M records/second.

**Delivery, observability, and security**
- **CloudFront:** over 2.1T HTTP requests during the week, up 5% from 2025.
- **CloudWatch:** over 2.15 quadrillion metric observations per day.
- **CloudTrail:** 3.6T events in four days, up 44%.
- **GuardDuty:** about 14T log events per hour monitored, up 59%.

## What the numbers suggest

- **Resilience testing is growing fastest.** The 6× jump in fault-injection experiments (44,000+) is the biggest year-over-year change in the post. Amazon is deliberately breaking things before peak traffic, at much larger scale than before.
- **Security and audit volume is growing faster than traffic.** CloudFront requests grew 5%, while GuardDuty (+59%) and CloudTrail (+44%) grew far more. The cost of watching the system is rising faster than the traffic itself.
- **Serverless and containers are where growth is.** Fargate grew about 48% year over year.
- **AWS's own chips are now a major share.** Graviton runs close to half of the retail site's compute.

The post ends with a pitch for AWS Countdown Premium, a paid AWS service to help customers prepare for their own traffic peaks.
