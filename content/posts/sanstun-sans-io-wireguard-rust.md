---
title: "sanstun: sans-IO Rust WireGuard where encryption runs on any thread"
slug: "sanstun-sans-io-wireguard-rust"
date: 2026-10-30T23:02:21+0000
summary: "Firezone released sanstun, a sans-IO userspace WireGuard implementation in Rust that replaces its boringtun fork (70+ patches over two years, now deprecated). It does no internal buffering or I/O, and its API separates bookkeeping from crypto: begin_encapsulate takes about 21 ns and returns a Send value whose encrypt step (about 934 ns for a 1280-byte packet) can run on a worker thread. Inbound packets can only enter through Interface::receive, which applies the rate limiter and cookie checks, so forgetting them is a compile-time impossibility. Apache 2.0, on crates.io."
source: "https://www.firezone.dev/blog/introducing-sanstun"
source_title: "Introducing sanstun: A sans-IO, parallel-friendly, userspace WireGuard implementation in Rust"
source_author: "Thomas Eizinger"
source_site: "firezone.dev"
source_date: "2026-10-30"
hn_url: "https://news.ycombinator.com/item?id=50310398"
---
Firezone has released **sanstun**, a Rust implementation of the WireGuard state machine built on the sans-IO pattern, which keeps I/O, time and randomness out of the protocol logic. It replaces Firezone's fork of boringtun, which is now deprecated.

## Why a rewrite

Firezone carried more than 70 patches on boringtun over two years. Keeping the fork compatible with upstream got harder as needs changed. Two examples:

- **Time:** making time handling sans-IO meant adding `_at` variants of APIs that take an `Instant`. That matters for accurately simulating a whole system.
- **Buffering:** boringtun auto-establishes sessions and buffers packets. Draining that buffer requires calling `decapsulate` with a zero-length datagram, it allocates, and the buffer size isn't tunable. sanstun doesn't buffer at all, so the caller decides what to do before a session exists.

## API design

- **One entry point for inbound packets.** Every datagram goes through `Interface::receive`, the only public API that parses raw bytes. It applies the rate limits, validates handshakes, and returns either a typed message or a cookie reply when under load. Constructors for message types like `HandshakeInitiation` are crate-private, so a `Tunnel` can't be handed a message that skipped the rate limiter. Misrouting a packet to the wrong tunnel is likewise ruled out by the types.
- **Bookkeeping split from crypto.** `begin_encapsulate` doesn't encrypt. It checks the session, reserves a packet counter and advances timers, then returns a `PendingEncapsulate` that is `Send`. Calling `.encrypt()` on it can happen on another thread. Decapsulation works the same way, with a final `commit_decapsulate` for the replay check.

## Numbers

From the repo's benchmark, bookkeeping costs about 21 ns (`begin_encapsulate`), 38 ns (`begin_decapsulate`) and 18 ns (`commit_decapsulate`), regardless of packet size. Encrypt and decrypt cost about 930 ns and 915 ns at 1280 bytes. So bookkeeping is roughly 2% of the work, which suggests a theoretical speedup above 45x from offloading crypto. The authors say real gains will be lower because of the cost of passing work between threads. You can also just call `encrypt` and `decrypt` inline if you don't need parallelism.

Keeping everything around `Interface` single-threaded lets the rate limiter run without locks or atomics. sanstun is Apache 2.0 licensed and available on crates.io.
