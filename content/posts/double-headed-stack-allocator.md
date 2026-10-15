---
title: "Double-headed stack allocator: two bump allocators in one block, no fragmentation"
slug: "double-headed-stack-allocator"
date: 2026-10-15T04:02:09+0000
summary: "A double-headed stack allocator splits one fixed memory block between two bump allocators: one grows up from the start, the other down from the end. Each side can reset independently, so two systems with different lifetimes (say streamed level data and a shape-shifting character) never fragment each other's memory, and the split between them stays flexible. Allocation is a single integer add."
source: "https://blog.demofox.org/2026/10/03/the-double-headed-stack-allocator/"
source_title: "The Double Headed Stack Allocator"
source_author: "Alan Wolfe (demofox)"
source_site: "The blog at the bottom of the sea"
source_date: "2026-10-03"
hn_url: "https://news.ycombinator.com/item?id=50098274"
---

**Bottom line:** A double-headed stack allocator puts two bump allocators in one fixed block of memory. One grows upward from address 0, the other grows downward from the end, and you're out of memory only when their tops of stack cross. Each side can be reset on its own timeline, which avoids the fragmentation you get when two systems allocate and free interleaved chunks from a shared pool.

**Background: the plain stack (bump) allocator.** Allocating is `ret = &mem[tos]; tos += size`, and freeing everything is `tos = 0`. The author used one around 2010 for CPU animation blending, where many transient allocations per frame were a profiler hot spot. Afterwards allocation no longer showed up in profiling. Caveats: destructors aren't run (placement new is easy, manual destruction is error-prone), so it suits simple types. It works across threads with an atomic add on `tos`, and you can do partial deallocation by saving and restoring `tos` for a recursive level.

**The fragmentation problem.** In a streaming game, level geometry and a shape-shifting player character allocate and free on separate schedules. In a 100 MB example, interleaved 20 MB and 30 MB allocations leave 40 MB free, but only as two separate 20 MB gaps, so a new 30 MB level allocation fails.

**The fix.** Give the level data the stack growing up and the character data the stack growing down. Either side can reset its `tos` and reallocate without fragmentation. Compared with two separate fixed-size allocators, the split between the two systems stays flexible, which matters when memory is tight and some levels need more while the character needs less. It still works multithreaded with atomic adds on either end.

**Variation: variable-sized objects with random access by index.** Use one stack for the objects and the other for an array of offsets into them. This fits, for example, packing a variable number of variable-sized network commands into a fixed-size packet. You need a count or a sentinel (such as all 1s) to mark the end of the offset table. The two updates don't need to be transactional. Items can also be removed or moved, as long as the offset table is updated.

The author also links the Vulkan Memory Allocator's linear algorithm for custom memory pools as a related write-up.
