---
title: "TanStack Start 1.0 is stable after SSR throughput rose about 5.5x"
slug: "tanstack-start-1-0-stable"
date: 2026-10-14T13:03:14+0000
summary: "TanStack Start, the full-stack React framework built on TanStack Router, has reached 1.0, which the team presents as a commitment that its APIs will not change out from under users. Creator Tanner Linsley describes it as a familiar SPA-style model where you choose per route whether to render on the server, load data on the server, or render on the client, with typed server functions and middleware crossing the client/server boundary."
source: "https://tanstack.com/blog/tanstack-start-1-0"
source_title: "TanStack Start 1.0 is Stable"
source_author: "Tanner Linsley"
source_site: "TanStack Blog"
source_date: "2026-10-13"
hn_url: "https://news.ycombinator.com/item?id=50081649"
---
**Bottom line:** TanStack Start 1.0 is out. Linsley says companies, including Fortune 500 teams, have already run it in production since the beta, and 1.0 is the formal promise that the APIs are stable.

## What the post says

- **Origin:** Start grew out of TanStack Router's end-to-end type safety (typed paths, params, search params, loader data and context). Server utilities appeared around December 2023, and the package was renamed `start` in May 2024.
- **Positioning:** An alternative to the server-first direction of Next.js and React Server Components. Components and routes stay familiar, and the server is used through explicit APIs such as `createServerFn` and `createIsomorphicFn`, which picks a server or client implementation and strips the other from the bundle.
- **Full-stack middleware:** Middleware can span client and server, with types carrying context through.
- **Performance:** Profiling the server and removing unneeded work gave roughly 5.5x SSR throughput in the team's benchmark. Contributors Manuel Schiller, Florian Pellet and Christopher Horobin are credited for that and for type-checking speed.
- **Frameworks and builds:** It is designed to be framework-agnostic (Solid support exposed React assumptions). It uses Vite and Nitro for deployment targets, and also supports Rsbuild.
- **RSC:** The team spent a lot of time on React Server Components support, aiming for them to fit into Start without owning the whole application model. Linsley is openly skeptical of how RSC was marketed.
