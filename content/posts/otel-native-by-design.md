---
title: "OTel-Native by Design: Building Products That Export to Any Observability Stack"
slug: "otel-native-by-design"
date: 2026-10-09T22:09:25+0000
summary: "The OpenTelemetry project advises product builders to let users push logs, traces and metrics over OTLP to an endpoint of their choice, rather than offering only built-in dashboards or polling APIs. Self-hosted software should ship pre-instrumented with endpoint config, while platforms should offer configurable export destinations."
source: "https://opentelemetry.io/blog/2026/otel-native-by-design/"
source_title: "OTel-Native by Design - Building Products That Export to Any Observability Stack"
source_author: "Nityananda Gohain and Dhruv Ahuja (SigNoz)"
source_site: "opentelemetry.io"
source_date: "2026-10-08"
hn_url: "https://news.ycombinator.com/item?id=50016974"
---
Users eventually want telemetry sent to their own observability stack for compliance, cost or consolidation. The post argues that supporting export to any OpenTelemetry-compatible backend is the vendor-neutral way to do it, and that OTLP push should be the default for new designs.

**What good looks like:** vendor-neutral (point at any Collector or OTLP backend), no custom integration work, rich context preserved (such as log records linked to trace IDs), and consistent use of OpenTelemetry Semantic Conventions. The same story applies to logs, traces and metrics; profiles entered public alpha in March 2026 and are not covered.

**Two contexts**
- **Self-hosted software** (Kuma, Keycloak): ship pre-instrumented and let customers set an endpoint through config or environment variables. Keycloak uses a single telemetry endpoint flag with per-signal toggles (logs are still in preview); Kuma uses separate policies per signal.
- **Platforms you operate** (Cloudflare Workers, Heroku): add a feature like Telemetry Drains where customers pick a destination, and your infrastructure forwards data. Heroku lets users choose signals, which helps control volume and cost. Cloudflare exports traces and logs but not yet metrics.

**Push versus polling:** polling APIs push pagination, retries and backfill onto users and make near-real-time delivery hard. OTLP push avoids that and works with any compatible backend.

**Practical advice:** accept an OTLP endpoint plus auth headers, let users toggle signals, use either the OTel SDK or an internal Collector, stick to standard OTEL_EXPORTER_OTLP_* variables, offer a single base URL for most users and per-signal endpoints for advanced ones, and keep attribute names consistent with Semantic Conventions (Weaver can help manage custom registries).
