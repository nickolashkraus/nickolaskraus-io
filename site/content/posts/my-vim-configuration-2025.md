+++
title = 'My Vim Configuration (2025)'
date = 2025-07-03T00:00:00-00:00
lastmod = 2025-08-30T00:00:00-00:00
draft = false
description = '''
A comprehensive walk-through of my Vim configuration and plugins that I use.
'''
tags = ['programming', 'vim']
+++

It's been a while since I've provided an update on my Vim configuration. *A
lot* has changed in the intervening 7 years.

The following provides a comprehensive walk-through of my Vim configuration and
plugins that I use.

## Configuration

My full Vim configuration file is [here][.vimrc].

## Plugins

### Plugin Manager

#### [vim-plug][vim-plug]
vim-plug is a minimalist and fast plugin manager for Vim and Neovim. As of
2025, vim-plug remains the most widely-used plugin manager across Vim and
Neovim, thanks to its speed, ease of use, and robust features.

### Language

#### [ale][ale]
ALE (Asynchronous Lint Engine) is a popular Vim plugin that provides real-time
linting and fixing capabilities.[^1] ALE is particularly popular because it
brings modern IDE-like features to Vim while maintaining the editor's
philosophy of being fast and customizable.[^2]

#### [vim-go][vim-go]
vim-go is a popular Vim plugin that provides comprehensive Go development
support within Vim. It's designed to make Vim feel like a full-featured IDE for
Go programming.[^3]

#### [tabular][tabular]
Tabular is a popular Vim plugin for text alignment and formatting. It's
designed to help you align text in columns, making it easier to create
well-formatted tables, code, and other structured text.[^4]

#### [vim-markdown][vim-markdown]
vim-markdown is a Vim plugin that enhances Markdown editing by adding features
that are not available in Vim by default.

#### [vim-anyfold][vim-anyfold]
vim-anyfold is a Vim plugin that provides automatic code folding functionality.
It's designed to intelligently fold code structures in any programming language
without requiring language-specific configuration.

#### [vim-polyglot][vim-polyglot]
vim-polyglot is a language pack plugin for Vim and Neovim that bundles syntax
highlighting, indentation, and filetype detection for hundreds of programming
and markup languages—all in one plugin.

### Completion

#### [auto-pairs][auto-pairs]
auto-pairs is a popular Vim plugin that automatically inserts or deletes
brackets, parentheses, and quotes in pairs.

#### [YouCompleteMe][YouCompleteMe]
YouCompleteMe (YCM) is a popular code completion engine for Vim. It provides
fast, intelligent autocompletion for multiple programming languages.[^5]

### Code Display

#### [gruvbox][gruvbox]
Gruvbox is a popular retro-inspired colorscheme for Vim and other text editors.
It's designed with warm, earthy tones that are easy on the eyes for long coding
sessions.

### Integrations

#### [vim-gitgutter][vim-gitgutter]
vim-gitgutter is a popular Vim plugin that visually integrates Git diff
information with the buffer's gutter (i.e., the sign column to the left of the
line numbers).

#### [fzf][fzf]
fzf is a general-purpose command-line fuzzy finder. It helps you quickly search
and select items from a list using a fast, interactive fuzzy search interface.
It's written in Go and works in any Unix-like terminal environment.[^6]

#### [fzf.vim][fzf.vim]
fzf.vim wraps fzf functionality into Vim commands and key mappings.

#### [gundo.vim][gundo.vim]
gundo.vim is a Vim plugin (originally by Steve Losh) that provides a visual
interface for navigating Vim's powerful but complex undo tree.

#### [vim-test][vim-test]
vim-test is a popular Vim plugin that provides a unified interface for running
tests from various testing frameworks directly within Vim. It's designed to
work across multiple programming languages and testing tools.

### Interface

#### [NERDTree][NERDTree]
NERDTree is a popular file system explorer plugin for the Vim text editor.  It
provides a tree-style navigation panel that displays your project's directory
structure in a sidebar, making it easier to browse and manage files without
leaving Vim.

#### [nerdtree-git-plugin][nerdtree-git-plugin]
nerdtree-git-plugin is an extension for NERDTree that shows Git status flags
for files and directories directly in the file tree.

#### [vim-airline][vim-airline]
vim-airline is a lightweight status/tabline plugin for Vim that enhances the
appearance and functionality of the default statusline.

#### [vim-airline-themes][vim-airline-themes]
vim-airline-themes is a companion plugin to vim-airline that provides a large
collection of predefined color themes specifically for the airline statusline.

### Commands

#### [vim-commentary][vim-commentary]
vim-commentary is a popular Vim plugin created by Tim Pope that provides simple
and intuitive commenting functionality. It allows you to easily comment and
uncomment lines or blocks of code in various programming languages.

#### [vim-fugitive][vim-fugitive]
vim-fugitive is a popular Vim plugin by Tim Pope that provides comprehensive
Git integration directly within Vim.

#### [vim-repeat][vim-repeat]
vim-repeat is a foundational Vim plugin by Tim Pope that enhances Vim's
built-in `.` (dot) command to work properly with plugin-defined operations.

#### [vim-surround][vim-surround]
vim-surround is one of Tim Pope's most popular Vim plugins that provides easy
manipulation of "surroundings" - parentheses, brackets, quotes, XML tags, and
more.

#### [vim-easy-align][vim-easy-align]
vim-easy-align is a Vim plugin by Junegunn Choi that provides powerful and
intuitive text alignment capabilities. It makes it easy to align text around
delimiters like `=`, `,`, `:`, `|`, and more.

### Other

#### [vim-sensible][vim-sensible]
vim-sensible is a foundational Vim plugin by Tim Pope that provides a minimal
set of sensible default settings for Vim. It's designed to be a starting point
that most Vim users would find reasonable, without being opinionated about more
subjective preferences.

#### [vitality.vim][vitality.vim]
vitality.vim is a Vim plugin (primarily by Steve Losh) designed to improve
the integration between Vim and terminal multiplexers, particularly tmux and
screen. It addresses various issues that arise when running Vim inside these
terminal environments.

[.vimrc]: https://github.com/nickolashkraus/dotfiles/blob/master/.vimrc
[vim-plug]: https://github.com/junegunn/vim-plug
[ale]: https://github.com/dense-analysis/ale
[vim-go]: https://github.com/fatih/vim-go
[tabular]: https://github.com/godlygeek/tabular
[vim-markdown]: https://github.com/preservim/vim-markdown
[vim-anyfold]: https://github.com/pseewald/vim-anyfold
[vim-polyglot]: https://github.com/sheerun/vim-polyglot
[auto-pairs]: https://github.com/jiangmiao/auto-pairs
[YouCompleteMe]: https://github.com/ycm-core/YouCompleteMe
[gruvbox]: https://github.com/morhetz/gruvbox
[vim-gitgutter]: https://github.com/airblade/vim-gitgutter
[fzf]: https://github.com/junegunn/fzf
[fzf.vim]: https://github.com/junegunn/fzf.vim
[gundo.vim]: https://github.com/sjl/gundo.vim
[vim-test]: https://github.com/vim-test/vim-test
[NERDTree]: https://github.com/preservim/nerdtree
[nerdtree-git-plugin]: https://github.com/Xuyuanp/NERDTree-git-plugin
[vim-airline]: https://github.com/vim-airline/vim-airline
[vim-airline-themes]: https://github.com/vim-airline/vim-airline-themes
[vim-commentary]: https://github.com/tpope/vim-commentary
[vim-fugitive]: https://github.com/tpope/vim-fugitive
[vim-repeat]: https://github.com/tpope/vim-repeat
[vim-surround]: https://github.com/tpope/vim-surround
[vim-easy-align]: https://github.com/junegunn/vim-easy-align
[vim-sensible]: https://github.com/tpope/vim-sensible
[vitality.vim]: https://github.com/sjl/vitality.vim

[^1]: ALE is the
[spiritual successor](https://github.com/vim-syntastic/syntastic#1-deprecation-note)
of Syntastic.
[^2]: I disable all LSP integrations via ALE. I use YouCompleteMe instead.
[^3]: I disable automatic linting via vim-go. I use ALE instead.
[^4]: I love this plugin, since I often found myself manually aligning table
columns.
[^5]: Though YouCompleteMe has some of the fixing and linting features offered
by ALE, I use YCM purely for code completion.
[^6]: fzf is an incredibly powerful tool, but has a steep learning curve.
