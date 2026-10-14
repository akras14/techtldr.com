---
title: "Go 1.27 HTTP/1 client runs the same loop 5x faster by draining bodies"
slug: "go-1-27-http1-connection-reuse"
date: 2026-10-14T18:02:19+0000
summary: "Go 1.27's net/http now automatically drains unread HTTP/1 response bodies (up to 256 KB or 50 ms) in the background, so connections get reused instead of abandoned. In the Go team's benchmark, 100 GET requests that only read the status code drop from about 710 ms on Go 1.26 to about 150 ms. You can go back to simply calling defer resp.Body.Close() without a hand-written drain helper."
source: "https://go.dev/blog/http-connection-reuse"
source_title: "Better HTTP/1 connection reuse"
source_author: "Nicholas Husin and Damien Neil"
source_site: "The Go Blog"
source_date: "2026-10-14"
hn_url: "https://news.ycombinator.com/item?id=50090369"
---

**Bottom line:** Through Go 1.26, closing an HTTP/1 response without reading the whole body made `net/http` throw the connection away, so every following request paid a fresh TCP and TLS handshake. Go 1.27 fixes this inside the transport. Under HTTP/1 against index.golang.org, 100 sequential requests that only check the status code take about 150 ms instead of about 710 ms.

## Why connections were lost

- HTTP/1 sends responses back-to-back on one byte stream, so the next response can only be found after the current body is fully read.
- Closing with unread bytes left the connection unusable, and Go dropped it.
- This hit code that ignores the body (status-only checks), and also code that stops one byte short. `json.Decoder` stops at the closing brace and leaves a trailing newline unread.
- HTTP/2 is not affected, because it multiplexes streams.

## What changes

- The transport now drains an unread HTTP/1 body asynchronously, for at most 256 KB or 50 ms, so the connection can go back to the idle pool.
- It skips the drain when the response has `Connection: close`, since that would be wasted work.
- The post notes the behavior may still evolve.

## What to do

- Delete hand-rolled `io.Copy(io.Discard, body)` helpers. The authors point out these were easy to get wrong (unbounded for large or slow bodies, pointless on HTTP/2).
- Just `defer resp.Body.Close()`.
