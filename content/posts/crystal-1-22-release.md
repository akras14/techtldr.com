---
title: "Crystal 1.22 exits on broken pipes, drops Process.fork, promotes aarch64 Linux"
slug: "crystal-1-22-release"
date: 2026-10-22T15:02:57+0000
summary: "Crystal 1.22.0 (251 changes, 33 contributors) has two breaking changes: processes now exit immediately when writing to STDOUT or STDERR hits EPIPE, and the long-deprecated Process.fork is removed. aarch64-linux becomes a Tier 1 platform, and DWARF parsing is rewritten for speed and lower memory."
source: "https://crystal-lang.org/2026/10/12/1.22.0-released/"
source_title: "Crystal 1.22.0 is released!"
source_author: "Johannes Müller"
source_site: "The Crystal Programming Language"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50194938"
---

Crystal 1.22.0 (251 changes, 33 contributors) has two breaking changes: processes now exit immediately when writing to STDOUT or STDERR hits EPIPE, and the long-deprecated Process.fork is removed. aarch64-linux becomes a Tier 1 platform, and DWARF parsing is rewritten for speed and lower memory.

## Breaking changes

- **Exit on pipe error:** a failed write to STDOUT/STDERR with EPIPE now ends the process, like the default SIGPIPE behavior, instead of raising IO::Error and printing a message. Opt out with `STDOUT.exit_on_broken_pipe = false`.
- **Process.fork removed:** it had been deprecated since before 1.0.
- Several HTTP return-value fixes (`HTTP::Request#body=`, `#content_length=`) are also marked breaking.

## New and improved

- Constants can carry type declarations (`BAR : Int32 = 1`).
- New compile-time macros: `StringLiteral#bytesize`, `NumberLiteral#chr`, and `RangeLiteral#each` over chars. New `Crystal::ALL_FLAGS` and `USER_FLAGS` constants expose compile flags.
- DWARF parsing was rewritten: on-demand section scanning, an index table, an LRU cache for decoded backtrace lines, and DWARF 5 form support.
- Every target now has a native event loop (epoll on Solaris, kqueue on remaining BSDs). libevent is opt-in and expected to be deprecated.
- Kernel TLS is enabled by default in the OpenSSL bindings.
- Many HTTP validation fixes: unrecognized Transfer-Encoding, invalid Content-Encoding and trailers are now rejected.
- LLVM 23 and 24 are supported, and there are first builds for aarch64-windows-msvc.
- Building Crystal 1.22 requires a stage-0 compiler at 1.14 or higher. Uppercase instance and class variables are deprecated.
