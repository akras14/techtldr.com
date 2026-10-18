---
title: "Free Pascal and Lazarus get dynamic packages: install components without rebuilding the IDE"
slug: "free-pascal-lazarus-dynamic-packages"
date: 2026-10-18T20:02:12+0000
summary: "A first release of full dynamic package support for the Free Pascal Compiler and Lazarus IDE lets you install components and IDE extensions in seconds instead of rebuilding the whole IDE. Packages are ordinary shared libraries (Linux, Windows, macOS) with compiler-generated metadata, and they can also serve as a hot-swappable plugin system in your own programs."
source: "https://www.getlazarus.org/dynamic/"
source_title: "Dynamic Packages"
source_site: "getlazarus.org"
hn_url: "https://news.ycombinator.com/item?id=50144066"
---

The author announces a complete dynamic package implementation for the Free Pascal Compiler and Lazarus, available in installers from getlazarus.org. The changes live in forked FPC and Lazarus branches named "dynamic packages" on GitLab.

- **How it works:** packages are standard shared libraries with extra compiler-generated metadata, queryable through new RTL units (including `DynPkgTypes.pas`). The runtime safely handles thread variables and dynamic types such as strings and dynamic arrays across program and package boundaries. You can load, enumerate, search and unload packages at runtime.
- **Deployment:** ship as one executable, or split into packages that can be replaced while the program runs, which gives you a plugin system exposing native Pascal classes.
- **IDE changes:** changing a registered package's source triggers an automatic rebuild before Lazarus starts, with a progress window. Adding or removing packages (including several at once) no longer requires an IDE rebuild. Compiling an installed package updates its components within a second or two. A new Packages window lists recent packages, and packages can be restricted to a platform or widgetset. A new feature converts any form or frame into an installable control on the component palette.
- **Restrictions:** a unit can't share its package's name (generated sources now append "Package", so `ElectroTools.lpk` yields `ElectroToolsPackage.pas`), unit names must be unique across packages, and dynamic packages are larger than ordinary shared libraries. Stripping them removes the metadata they need.
