---
title: "Bevy 0.20"
slug: "bevy-0-20"
date: 2026-10-09T22:09:25+0000
summary: "Bevy 0.20, the Rust game engine, ships with a faster and more accurate Solari path tracer (now on macOS via Metal), reworked BSN scene syntax that drops most wrapper boilerplate, new editor-style UI widgets, official adoption of the WESL shader language, and custom sprite materials. The release came from 227 contributors and 817 pull requests."
source: "https://bevy.org/news/bevy-0-20/"
source_title: "Bevy 0.20"
source_author: "Bevy Contributors"
source_site: "bevy.org"
source_date: "2026-10-08"
---
Bevy 0.20 is a feature release with changes in rendering, scenes, UI and shaders. It comes with a 0.19 to 0.20 migration guide.

**Highlights**
- **Solari and DLSS:** Bevy's realtime path-traced renderer is faster, more accurate and supports more rendering features, and it now runs on macOS via Metal (without a built-in denoiser there for now). ReSTIR is now optional and off by default, because DLSS ray reconstruction has become good enough that ReSTIR often costs more performance than it adds in quality. Expect weaker shadows in scenes with many lights unless you re-enable it. Solari also supports Atmosphere and environment-map lighting, and dlss_wgpu now supports DLSS-RR 4.5, which requires updating the DLSS SDK.
- **BSN syntax:** The new scene system gets ergonomic changes the authors say should mostly settle the syntax. Scene references need an @ prefix, the template_value wrappers are no longer needed, enums work without VariantDefaults (but every field must be specified), chained builder methods work directly, and entities in a list are separated with -- instead of commas. BSN scenes also fire an observable Ready event once all children have spawned.
- **UI:** Bevy Feathers, the editor-centric toolkit, adds color input, scrollable list view, dropdown and lazy menu widgets, a scrubbable number input, and headless tab widgets.
- **Shaders:** Bevy officially adopts WESL, a standardized extension of WGSL, replacing its custom WGSL dialect.
- **2D and cameras:** Custom shader materials for sprites, extended 2D mesh materials, and a CAD-style pan-orbit camera.

The release notes also list smaller items such as weak system ordering with chain_weak, schedule randomization, panic catching, faster bulk despawning, better texture compression and richer error context.
