---
title: "Type Server Protocol (TSP): JSON-RPC for asking Pylance for Python types"
slug: "pylance-type-server-protocol"
date: 2026-10-26T19:03:00+0000
summary: "Microsoft's Pylance team published a draft spec for the Type Server Protocol (TSP), a JSON-RPC protocol that lets a client ask a Python type server for computed, declared and expected types, import resolution and search paths. It reuses LSP conventions and is meant to run alongside an LSP connection, so other tools can reuse the type checker's analysis instead of reimplementing it. It is still version 0.x, where minor bumps may break compatibility."
source: "https://github.com/microsoft/pylance-release/blob/main/docs/tsp/type-server-protocol.md"
source_title: "Type Server Protocol"
source_author: "Microsoft"
source_site: "github.com"
hn_url: "https://news.ycombinator.com/item?id=50248371"
---
The Type Server Protocol (TSP) is a JSON-RPC protocol for asking a Python type server about analysis results. The server keeps the type-analysis state for a workspace and answers requests under the `typeServer/` prefix. The spec is still in the `0.x` line (currently `0.4.1`), so minor version changes may be breaking.

**How it works**

- TSP borrows LSP data shapes (`Position`, `Range`, string URIs, zero-based UTF-16 offsets) and typically shares a connection with a normal LSP session, which supplies initialization and document sync.
- A client checks the protocol version, fetches a snapshot id, then sends type queries: computed, declared and expected type, import resolution, and Python search paths.
- Results are tied to a snapshot, an integer identifying a point-in-time view of analysis state. The server advances it whenever earlier results may be invalid (file edits, config changes, dependency changes) and sends `typeServer/snapshotChanged`. Requests carrying a stale snapshot fail with LSP's `ServerCancelled` (`-32802`) so the client can retry.
- Type results are tagged objects with a numeric `kind`, `id` and `flags`, covering built-ins, functions, classes, unions, modules, type variables, overloads and synthesized types.

**Optional multi-connection mode**

For high-volume type queries, client and server can negotiate extra read-only channels through an `experimental.typeServerMultiConnection` capability in `initialize`. The only transport so far is `ipc`. The main connection keeps all state-changing traffic (lifecycle, document sync, snapshot notifications, connection control), and extra connections may carry only the read-only TSP requests allowed for the negotiated version. Servers must still work correctly in single-connection mode.
