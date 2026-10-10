---
title: "Doom ported to SQL: 5,900 lines of game logic, up to 60 FPS on a laptop"
slug: "doom-in-sql"
date: 2026-10-10T21:31:50+0000
summary: "A CedarDB engineer ported the original 1993 Doom's game logic and renderer to SQL running inside the database. Python only reads the keyboard, ticks the clock and shows the frame. Game tics stay at Doom's 35 Hz, frames render at about 60 FPS on a laptop, and the database's transactions and access control give four-player deathmatch almost for free."
source: "https://cedardb.com/blog/sqldoom/"
source_title: "We ported the original Doom to SQL"
source_author: "Lukas Vogel"
source_site: "CedarDB"
hn_url: "https://news.ycombinator.com/item?id=49948300"
---

A CedarDB engineer ported the original 1993 Doom's game logic and renderer to SQL running inside the database. Python only reads the keyboard, ticks the clock and shows the frame. Game tics stay at Doom's 35 Hz, frames render at about 60 FPS on a laptop, and the database's transactions and access control give four-player deathmatch almost for free.

## The rules

Lukas Vogel's earlier DOOMQL used raycasting and looked more like Wolfenstein 3D. This time the goal was real Doom: the renderer must output an exact RGB bitmap from SQL, the game loop must be SQL (user-defined functions allowed), and the client may only handle input, timing and display.

## Game data as tables

The WAD file maps naturally to relational tables; importing all of Doom 1 takes about 1,000 lines of Python and 18 seconds. Weapons, monster states and even animations are rows, so you can change the shotgun to fire 500 pellets with an `UPDATE`.

## Game logic

Each tic runs a procedural function that triggers batches of SQL statements. The logic is about 5,900 lines of SQL, versus roughly 9,000 lines of C in the original. It works like an entity-component system: each component is a table, each system a join. The worst tic found (46 monsters awake in E4M1) took 10.45 ms, 37% of the 28.6 ms budget; a typical tic takes about 2.15 ms.

## Renderer

The renderer is one big view: about 1,300 lines of SQL across 89 CTEs (Doom's C renderer is about 3,300 lines). Highlights:

- **BSP ordering:** every root-to-subsector path is precomputed and packed into a bigint, so sorting gives front-to-back order; bounding boxes cull invisible branches.
- **Walls** expand into per-column, per-pixel rows, at about 1.7 ms.
- **Floors and ceilings:** Doom's imperative clipping is emulated with window functions over near-to-far wall slices.
- **Depth resolve:** instead of Doom's careful ordering, it generates every candidate pixel and picks the nearest by packing depth and color into a key and taking the min. It's the costliest step, which is exactly why Carmack avoided it.

On a Ryzen 7 7840U it typically runs at about 60 FPS, dropping to 35 FPS in busy scenes.

## Where a database helps

- **Multiplayer:** each tic is a transaction, so all four players see either the old or the new world, never half an update.
- **Access control:** player roles can only call a few API functions, and the input function clamps values, so clients aren't trusted. About three cores per client hold a stable 35 FPS.
- **Generated code:** CedarDB compiles queries to machine code through LLVM. For one movement routine, the C version compiles to 48 instructions; the SQL version has more, but 42 of the extra instructions are writing results back to a table.

The code is open source, and there's a public deathmatch server where you can also query live game state.
