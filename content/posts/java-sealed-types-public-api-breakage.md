---
title: "Java sealed types in a public API turn new subtypes into runtime failures"
slug: "java-sealed-types-public-api-breakage"
date: 2026-10-18T17:02:27+0000
summary: "Sealed types give Java compile-time exhaustiveness checking, but only when every consumer is recompiled. Neil Madden concludes they work best inside a component, and are risky in a library's public API, where adding a subtype is a breaking change that may only show up at runtime."
source: "https://neilmadden.blog/2026/04/24/java-sealed-classes-and-exhaustive-pattern-matching/"
source_title: "Java sealed classes and exhaustive pattern matching"
source_author: "Neil Madden"
source_site: "Neil Madden"
source_date: "2026-04-24"
hn_url: "https://news.ycombinator.com/item?id=50141579"
---

**Bottom line:** Sealed types let a Java `switch` skip the `default` branch because the compiler knows every case. Madden's point is that this guarantee only holds when consumers are recompiled. Exposed in a public API, a sealed type turns "add a subtype" into a breaking change.

- **Compile time:** if you add a `TypeC` to a sealed interface and recompile the consumer, a switch missing that case fails to build. That is the intended safety net, but it also means a new subtype needs a major version bump under SemVer.
- **Runtime:** if only the sealed type is recompiled and the consumer is not, the old switch compiles to code that throws `MatchException` when the new case appears.
- **Conclusion:** use sealed types inside a component, where everything is built together. Across module or library boundaries they mostly add new ways to break callers, often detectable only at runtime.

Madden noticed this while reviewing the preview PEM-encoded cryptographic objects API in the JDK, which bakes a sealed interface into the public API and tells clients to pattern match on it. He expects breakage if a new case is ever added.
