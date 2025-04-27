---
title: "Vim Plugins That I Use - 2024"
date: 2023-12-08T00:00:00-06:00
draft: true
description: An updated guide to the Vim plugins that I use.
tags: ["programming", "vim"]
---

It's been six years since my original [post](https://nickolaskraus.io/articles/vim-plugins-that-i-use-2018/), so I thought I would provide an update on the Vim plugins that I use. This time around, plugins will be organized into the following catagories (similar to [VimAwesome](https://vimawesome.com)):
* Language
* Completion
* Code Display
* Integrations
* Interface
* Commands
* Other

## Language

### [dense-analysis/ale](https://github.com/dense-analysis/ale)

>ALE (Asynchronous Lint Engine) is a plugin providing linting (syntax checking and semantic errors) in NeoVim 0.6.0+ and Vim 8.0+ while you edit your text files, and acts as a Vim [Language Server Protocol](https://langserver.org/) client.

Previously, I used [Syntastic](https://github.com/vim-syntastic/syntastic) for syntax checking, however the project was deprecated on [July 5th, 2022](https://github.com/vim-syntastic/syntastic/commit/6f638ed5bb214213a788c6c1aaa565937efd5e8c). The recommended alternative is [ALE](https://github.com/dense-analysis/ale).

ALE is *fast* and allows you to lint while you type, providing out-of-the-box support for a huge array of linters and language servers (the full list of languages and tools can be found [here](https://github.com/dense-analysis/ale/blob/master/supported-tools.md)).

ALE can also be used for autocompletion and *go to definition* type behavior, however I do not use this functionality.

### [tpope/vim-surround](https://github.com/tpope/vim-surround)

>Surround.vim is all about "surroundings": parentheses, brackets, quotes, XML tags, and more. The plugin provides mappings to easily delete, change and add such surroundings in pairs.

I love this plugin and use it religiously.

```bash
Hello, World!
```

`yss"`

```bash
"Hello, World!"
```

`cs"<q>`

```bash
<q>Hello, World!</q>
```

### [preservim/nerdtree](https://github.com/preservim/nerdtree)

>The NERDTree is a file system explorer for the Vim editor. Using this plugin, users can visually browse complex directory hierarchies, quickly open files for reading or editing, and perform basic file system operations.

A dedicated file system explorer is standard in almost all IDEs. Though vanilla Vim can be used to navigate the filesystem, NERDTree makes it intuitive and simple. It can also be extended with custom mappings and functions using its built-in API (see [Xuyuanp/nerdtree-git-plugin](https://github.com/Xuyuanp/nerdtree-git-plugin)).

netrw diectory listing


### [fatih/vim-go](https://github.com/fatih/vim-go)

>This plugin adds Go language support for Vim...

This is an absolute necessity when writing Go.

**NOTE**: Install all necessary binaries for vim-go with `:GoInstallBinaries`.

## Completion

### [ycm-core/YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)




## Code Display

## [morhetz/gruvbox](https://github.com/morhetz/gruvbox)

I love this colorscheme. I basically use it for everything. See the cover art for an example.


Incremental search


## Other

### [preservim/nerdcommenter](https://github.com/preservim/nerdcommenter)

>Comment functions so powerful—no comment necessary.

This plugin allows you to quickly comment or uncomment one or more lines with a single command.

**NOTE**: Make sure that you have filetype plugins enabled, as the plugin makes use of `|commentstring|` where possible, which is usually set in a filetype plugin:

```
filetype plugin on
```

Harpoon

Treesitter (undo tree)?
