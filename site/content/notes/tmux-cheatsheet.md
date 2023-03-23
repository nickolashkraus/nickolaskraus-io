---
title: "tmux Cheatsheet"
date: 2018-10-21T00:00:00-06:00
draft: false
description: Helpful commands for tmux
tags: ["programming", "tmux"]
---

Use the prefix `^Ctrl-a`. This is the most ergonomic combination, offering the least ulnar deviation. It does require, however, that the bash shortcut for moving the cursor to the start of the line (`^Ctrl-a`) be executed twice. This is not applicable if using the `vi-mode` plugin for Zsh, which allows Vim-like motions on the command-line.

```bash
tmux new -s <session>
```

```bash
tmux attach -t <session>
```

```bash
tmux ls
```

```bash
tmux kill-server
```

## How to swap panes

The `swap-pane` command can do this for you. The `{` and `}` keys are bound to `swap-pane -U` and `swap-pane -D` in the default configuration.

### How to copy text with a vertical split

Use `<prefix>-z` to expand (*zoom*) the current pane. Copy the needed text. Use `<prefix>-z` to return the pane to its original size.
