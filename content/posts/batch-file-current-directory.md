---
title: "Program works double-clicked but fails from a batch file: set the current directory"
slug: "batch-file-current-directory"
date: 2026-10-26T22:02:43+0000
summary: "Explorer sets a program's current directory to the folder containing the file you double-click, but a batch file that launches the program by full path leaves the current directory unchanged. Programs that look for companion files relative to their working directory then fail. Raymond Chen's fix: have the batch file switch to the program's directory first (pushd before, popd after)."
source: "https://devblogs.microsoft.com/oldnewthing/20261026-00/?p=112797/"
source_title: "Puzzling out why a program doesn't work when run from a batch file, but works when double-clicked from Explorer"
source_author: "Raymond Chen"
source_site: "The Old New Thing"
source_date: "2026-10-26"
hn_url: "https://news.ycombinator.com/item?id=50251070"
---

**Bottom line:** if a program works when double-clicked but errors when started from a batch file, suspect the current directory.

- Explorer sets the new process's current directory to the folder containing the double-clicked file (including an .exe). Programs that find ancillary files relative to the current directory rely on that.
- A batch file that runs the program by full path doesn't change directory, so those files aren't found.
- A customer confirmed the fix: `pushd` to the program's directory, run it, then `popd` to restore the original.
- Bonus: a second thing to check is whether the program registers a custom entry under App Paths to extend PATH when launched.
