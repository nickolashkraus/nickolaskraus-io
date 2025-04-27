---
title: "Neovim From Scratch"
date: 2023-12-06T00:00:00-06:00
draft: true
description: A guide for configuring Neovim from scratch
tags: ["programming", "vim", "neovim"]
---

At the end of 2023, I started revamping my local development setup. This led to my asking the inevitable question: *Should I consider using Neovim instead of Vim?*. After a little digging, I decided to make the switch.

If you use Vim or Neovim as your text editor, you are probably like me and want to know how things work *deeply*. For this reason, this guide will teach you *how* to use and configure Neovim. In my reasearch of Neovim, I came across several distributions:

* [NvChad](https://nvchad.com/) [GitHub](https://github.com/NvChad/NvChad)
* [LunarVim](https://www.lunarvim.org/) [GitHub](https://github.com/LunarVim/LunarVim)
* [LazyVim](https://lazyvim.github.io/) [GitHub](https://github.com/LazyVim/LazyVim)
* [AstroNvim](https://astronvim.com/) [GitHub](https://github.com/AstroNvim/AstroNvim)
* [nyoom.nvim](https://github.com/nyoom-engineering/nyoom.nvim)

Quite frankly, these distribution were too big, too much to grok, and, frankly, garish (Nerd Font is not my a e s t h e t i c).

The following were more my speed:

* [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim)

## What is Neovim?

Neovim is a fork of Vim that strives to improve the extensibility and maintainability of Vim. The Neovim project was started in 2014, after a patch to Vim supporting multi-threading was rejected:

>User demand for a multi-threaded Vim (e.g. a [patch](https://groups.google.com/forum/#!topic/vim_dev/65jjGqS1_VQ) for supporting multiple threads) and its rejection is what prompted the [Neovim fork](https://groups.google.com/forum/#!topic/vim_dev/x0BF9Y0Uby8) in January 2014.<sup>[1]</sup>

## Neovim vs. Vim

The original bifurcation of the project allowed Neovim to add built-in support for asynchronous I/O. This allowed operations to run in the background without blocking the editor. However, with [Vim version 8](https://raw.githubusercontent.com/vim/vim/master/runtime/doc/version8.txt), Vim added support for asynchronous I/O as well.

Currently, two of the main features of Neovim are built-in Language Server Protocol (LSP) support and support for Lua scripting using LuaJIT language interpreter. A full list of the differences between Neovim and Vim can be found [here](https://neovim.io/doc/user/vim_diff.html).

## Installation

You can install Neovim by several means. Since I use macOS, let's install it via Homebrew:

```bash
brew install neovim
```

Now to start Neovim, just run `nvim`.

**NOTE**: You can also alias `nvim` to `vim` with `alias nvim='vim'`.

## Using Lua in Neovim

One of the primary differences between Neovim and Vim is Neovim's support for Lua. The following will provide a super abbreviated guide to using Lua in Neovim. A complete guide for using Lua with Neovim can be found [here](https://neovim.io/doc/user/lua-guide.html#lua-guide).

**NOTE**: Additional context is provided around the API, using Lua files on startup, and Lua modules, but for the most part, the official documentation is spot on.

### Introduction

Lua is used to configure and modify Nvim. This is accomplished by interacting with three API layers:
1. **Vim API**: Inherited from Vim. This includes [Ex commands](https://neovim.io/doc/user/vimindex.html#Ex-commands) and [builtin functions](https://neovim.io/doc/user/builtin.html) as well as [user functions](https://neovim.io/doc/user/eval.html#user-function) in Vimscript. These are accessed through `vim.cmd` and `vim.fn` respectively.
2. **Nvim API**: Written in C for use in remote plugins and GUIs. These functions are accessed through `vim.api`.
3. **Lua API**: Written in and specifically for Lua. These are any other functions accessible through `vim.*` not mentioned already.

### Using Lua

To run Lua code from the Nvim command line, use the `:lua` command:

```
:lua print("Hello, World!")
```

Use `:lua=` (equivalent to `:lua vim.print(...)`) to check the value of a variable or table:

```
:lua =package
```

To run a Lua script in an external file, use the `:source` command:

```
:source ~/foo.lua
```

#### Using Lua files on startup

Nvim supports using `init.vim` or `init.lua` as the configuration file, but not both. This file should be placed in your configuration directory (ex. `~/.config/nvim`). Nvim will also source Ex commands (Vimscript) and/or Lua code (`.lua` files) found at specific locations automatically on startup. For example, Nvim will load all files matching the following file pattern:

```
plugin/**/*.{vim,lua}
```

**NOTE**: If two `.vim` and `.lua` file names match and differ only in extension, the `.vim` file is sourced first.

For information on the full initialization process, see [`:help startup`](https://neovim.io/doc/user/starting.html#startup)

It should be noted that Nvim will search a list of directories for additional runtime files. See [`:help runtimepath`](https://neovim.io/doc/user/options.html#runtimepath).

#### Lua modules

If you want to load Lua files explicitly, you can place them in the `lua/` directory in your 'runtimepath' and load them with `require()`. Use can then load files or modules directly:

So, given the following directory structure:

```
.
├── init.lua
└── lua
    ├── my_module
    │   ├── config.lua
    │   └── init.lua
    └── script.lua
```

The following will load the `script.lua` file:

```lua
--loads script.lua
require("script")
```

**NOTE**: The `.lua` extension is not included.

The following will load the `config.lua` file in the `my_module` module:

```lua
--loads my_module/config.lua
require("script")
```

The following will load the `my_module` module:

```lua
--loads my_module/init.lua
require("my_module")
```

For more information on loading Lua modules, see [`:help lua-module-load`](https://neovim.io/doc/user/lua.html#lua-module-load).

## Getting Started

Vim is configured using a `.vimrc` file. For Neovim user configuration is found in the following directories:
* `$XDG_CONFIG_HOME/nvim`: Configuration directory
* `$XDG_CONFIG_HOME/nvim/init.vim|init.lua`: Initialization configuration (can be either Vimscript (`init.vim`) or Lua (`init.lua`), but not both)

Let's create these:

```bash
$ mkdir $XDG_CONFIG_HOME/nvim
$ touch $XDG_CONFIG_HOME/nvim/init.lua
```

Next, we create a `lua/` directory. A typical Lua module has the following structure:

```
.
├── init.lua
└── lua
    ├── module_x
    │   └── init.lua
    ├── module_y
    │   └── init.lua
    └── module_z
        └── init.lua
```

When a Lua module (directory) is required/imported, Lua automatically looks for an `init.lua` file inside that directory. So, calling `require("module_x")` from `init.lua` will import the module in `lua/module_x`. The `init.lua` file is executed to initialize the module and often contains the main logic or entry point for the module.

## Project Structure

With Vim, it was typical to have one's entire configuration in a single `.vimrc` file. Neovim, on the other hand, allows for a more modular project structure facilitated by Lua modules.

## Plugins

Plugin Manager
- lazy

**NOTE**: As of August 2023, packer is no longer maintained.
https://github.com/wbthomason/packer.nvim

Fuzzy Finder

Tree-Sitter (AST, highlighting)

Undo Tree

nvim/after

LSP

mason (installs LSPs)

mason and mason-lspconfig

https://github.com/ThePrimeagen/harpoon/tree/harpoon2

https://en.wikipedia.org/wiki/Vim_(text_editor)#Neovim

telescope

Auto complete
hrsh7th/nvim-cmp

vim-surround
nvim-lsp + nvim-cmp
commenting
lualine
nvim-lsp
tabline bufferline
autosave
nvim-notify
wilder
whichkey

## Configuration

### Yank to clipboard
https://neovim.io/doc/user/provider.html#provider-clipboard


`%`: Create new file
`d`: Create new directory
`Ex`: Open explorer
`so`: Source current file
`=ap`: Auto-indenting around paragraph

## Conclusion

It is a sad truth that Vim and Nvim are going on quite different paths: Neovim is investing more on Lua, and its Lua plugin system is thriving, while Vim is trying hard implement the vim9 script, a completely new and incompatible version of viml. There might be more incompatible changes happening on both sides. It might be getting harder to port Vim patches to Neovim. Personally, I doubt whether the amount of effort put into building vim9 script will ever pay off. Considering that a lot of vim plugins are still trying to support Vim 7.xxx, I doubt how many of these plugin authors are willing to rewrite their plugins in vim9 script, especially when their plugins have very large code bases.

---
<sup>1</sup>: Vim's 25th anniversary and the release of Vim 8](https://lwn.net/Articles/713114/)

https://github.com/nvim-lua/kickstart.nvim
https://nvchad.com/
https://github.com/NvChad/ui
https://ignore.pl/how_to_organize_your_lua_project.html

https://github.com/wbthomason/packer.nvim
https://github.com/nvim-lua/kickstart.nvim
https://github.com/LunarVim/Neovim-from-scratch
https://github.com/VapourNvim/VapourNvim
https://github.com/rockerBOO/awesome-neovim
https://github.com/ngscheurich/nvim-from-scratch
https://www.youtube.com/playlist?list=PLhoH5vyxr6Qq41NFL4GvhFp-WLd5xzIzZ
