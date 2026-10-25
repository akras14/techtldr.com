---
title: "Wide events replace metrics, logs and traces: Meta's Scuba model (2024)"
slug: "wide-events-instead-of-metrics-logs-traces"
date: 2026-10-25T20:06:00+0000
summary: "Ivan Burmistrov, an ex-Meta engineer, argues that observability is needlessly confusing because OpenTelemetry presents metrics, logs and traces as separate things. At Meta, the Scuba system stored only wide events: JSON-like records with as many fields as possible, queried by slicing and grouping. Traces, logs and metrics are all special cases of such events, and the model handles unknown unknowns without any cardinality limits."
source: "https://isburmistrov.substack.com/p/all-you-need-is-wide-events-not-metrics"
source_title: "All you need is Wide Events, not “Metrics, Logs and Traces”"
source_author: "Ivan Burmistrov"
source_site: "A Song Of Bugs And Patches"
source_date: "2024-02-15"
hn_url: "https://news.ycombinator.com/item?id=50234329"
---

**Bottom line:** Burmistrov's claim is that "three pillars" (metrics, logs, traces) is the wrong mental model. One primitive, the wide event, covers all three and makes incident investigation easier. He blames OpenTelemetry's framing (and its 60+ term glossary) for making observability look more complicated than it is.

**What a wide event is.** A record of named fields, like a JSON document, written for any fact worth recording (for example an ad impression with country, placement, OS, OS version and app version). You are encouraged to dump in every field that might matter later, which prepares you for "unknown unknowns".

**How Scuba (Meta's system) uses them.** Events carry a sampling rate, and the UI upscales results automatically, so dynamic sampling is invisible to users. During an incident you pick a metric, then group and filter by arbitrary fields. In his example an impressions drop narrows to one campaign type, then Android, then the newest OS version and certain app versions, with no prior knowledge of the system and no pre-built metric. Because raw events are stored rather than pre-aggregated, field cardinality isn't a problem. He credits Scuba's explorable UI (field values and descriptions are browsable) over proprietary query languages or SQL.

**Everything maps onto it.**
- Traces: wide events with SpanId, TraceId and ParentSpanId; sort by parent links to draw the trace view.
- Logs: put the message in a field plus metadata; hashing a message template with IDs stripped lets you group the most frequent errors.
- Metrics: emit a state-snapshot event at an interval, much as Prometheus scraping does, without cardinality worries.
- Beyond that, continuous profiling data can be stored the same way, and having everything in one store helps cross-correlation and AI-assisted root-cause analysis.

He names Honeycomb and Axiom as vendors offering this simpler interaction model, and says the post is mostly an expression of frustration rather than a recipe. It is a 2024 opinion piece based on his Meta experience.
