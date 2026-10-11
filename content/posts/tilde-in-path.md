---
title: "A quoted ~ in PATH becomes a literal ./~ directory, not your home"
slug: "tilde-in-path"
date: 2026-10-11T14:03:00+0000
summary: "Writing export PATH=\"$PATH:~/.local/bin/\" in a shell rc file does not add your home directory's bin folder. Bash only expands ~ when it is unquoted, so the quoted version leaves a literal ~ in PATH, which resolves to ./~/.local/bin relative to whatever directory you are in. The author found it when the nono agent-sandboxing tool warned about a writable PATH entry. The fix is to use $HOME instead of ~, and a quick check is echo \"$PATH\" | tr ':' '\\n' | grep '~'."
source: "https://disconnect3d.pl//2026/10/02/dont-put-tilde-in-your-path/"
source_title: "The tilde in your PATH may not be your HOME"
source_author: "disconnect3d"
source_site: "disconnect3d.pl"
source_date: "2026-10-02"
hn_url: "https://news.ycombinator.com/item?id=50042425"
---

**Bottom line:** `export PATH="$PATH:~/.local/bin/"` does not put your home directory's `bin` on PATH. Tilde expansion only happens in unquoted words, so the quoted `~` stays literal and PATH gets an entry that resolves relative to the current directory (`./~/.local/bin/`). Use `$HOME` instead.

**How it was found.** The author ran the nono agent-sandboxing tool, which warned about a PATH entry the sandbox could write to. That led to the literal `~` in their shell config.

**Demo.** They created `./~/.local/bin/`, compiled a small program into it, and ran it with `PATH="~/.local/bin/" kek`. The binary ran, so the lookup went through the local `./~` directory and never touched the home directory.

**Details.**
- The unquoted form `export PATH=$PATH:~/.local/bin` does work in Bash and Zsh, because they expand tildes after `=` and after each `:` in assignments. The author calls relying on that fragile; for example, whitespace breaks the assignment.
- Check yours with `echo "$PATH" | tr ':' '\n' | grep '~'`. Any output means a bad entry in `.bashrc`, `.zshrc` or `.profile`.
- Beyond a broken PATH, the author's concern is that a relative PATH entry depends on the working directory. They also credit nono for the warning and say they plan a PR to make it more verbose.
