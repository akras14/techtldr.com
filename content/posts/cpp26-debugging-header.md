---
title: "C++26 adds <debugging> with std::breakpoint and is_debugger_present"
slug: "cpp26-debugging-header"
date: 2026-10-16T21:02:48+0000
summary: "C++26 standardizes programmatic breakpoints with a new <debugging> header (P2546R5, P2810R4) containing three noexcept, freestanding functions: std::breakpoint(), std::is_debugger_present(), and std::breakpoint_if_debugging(). It replaces the per-compiler #ifdef ladders (__debugbreak, __builtin_debugtrap, int3, raise(SIGTRAP)) that Boost.Test, Catch2, Dear ImGui and Unreal each reimplemented."
source: "https://www.sandordargo.com/blog/2026/09/30/cpp26-debugging"
source_title: "C++26: the new debugging header"
source_author: "Sandor Dargo"
source_site: "Sandor Dargo's Blog"
source_date: "2026-09-30"
hn_url: "https://news.ycombinator.com/item?id=50122736"
---

C++26 standardizes programmatic breakpoints with a new `<debugging>` header, proposed in P2546R5 with the companion P2810R4 (by Rene Ferdinand Rivera Morell). It replaces the platform-specific `#ifdef` ladders every C++ project has been writing: `__debugbreak()` on MSVC, `__builtin_debugtrap()` on Clang, inline `int3` on GCC/x86, and `raise(SIGTRAP)` elsewhere. Boost.Test, Catch2, Dear ImGui and Unreal each carry their own version of that macro today.

The header adds three functions, all `noexcept` and freestanding, none `constexpr`:

- **`std::breakpoint()`** halts in the debugger if one is attached. Without a debugger the behavior is implementation-defined; on most platforms the process gets a signal like SIGTRAP and terminates unless it's caught.
- **`std::is_debugger_present()`** reports whether the program is currently being traced. It's checked on every call, not cached, since a debugger can attach or detach at any time. The mechanism is up to the implementation (for example `IsDebuggerPresent()` on Windows, or reading `TracerPid` from `/proc/self/status` in libstdc++).
- **`std::breakpoint_if_debugging()`** is the convenience form: break only if a debugger is present, otherwise do nothing.

`is_debugger_present()` is a replaceable function, like `operator new`. P2810R4 added this for bare-metal targets that have no OS-level way to detect a debugger, so you can define it yourself and wire it to a hardware flag instead of excluding it from freestanding builds.

Beyond conditional breakpoints, the author suggests using the presence check to add extra diagnostics, disable timeouts during stepping, or switch to a more debugger-friendly allocator.
