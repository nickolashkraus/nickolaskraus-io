+++
title = 'Automating My MacBook Setup'
date = 2025-09-04T00:00:00-00:00
lastmod = 2025-09-04T00:00:00-00:00
draft = false
description = '''
A comprehensive walk-through of how I automated my entire MacBook setup.
'''
tags = ['technology', 'programming', 'macos']
+++

If you want instant 10x engineer aura, then you need to have your local
development setup fully automated[^1].

In this post, I'll provide a comprehensive walk-through of how I automated my
MacBook setup and my general philosophy around developer productivity.

I have three repositories that facilitate my local development setup
/ productivity:

1. [nickolashkraus/dotfiles][nickolashkraus/dotfiles]: My personal dotfiles
2. [nickolashkraus/bash-scripts][nickolashkraus/bash-scripts]: A collection of
   helpful Bash scripts 
3. [nickolashkraus/nhk-mac][nickolashkraus/nhk-mac]: A CLI for quickly setting
   up a new macOS workstation 

nhk-mac is the core application[^2], which utilizes bash-scripts and dotfiles
for configuration.

To setup a new macOS workstation, all I have to do is:

1. Open Terminal.

2. Download the zip file.

```bash
curl -L https://github.com/nickolashkraus/nhk-mac/archive/master.zip -o nhk-mac.zip
unzip -q nhk-mac.zip
mv nhk-mac-master nhk-mac
rm nhk-mac.zip
cd nhk-mac
```

3. Run `nhk-mac`.

```bash
./nhk-mac
```

This application does the following:
* Checks System Integrity Protection (SIP) status (yabai requires SIP to be
  disabled)
* Sets hostname
* Configures keyboard
* Configures Dock
* Configures System Settings
* Installs Xcode
* Installs Homebrew
* Installs Homebrew packages
* Installs Oh My Zsh
* Installs vim-plug
* Installs Powerline Fonts
* Generates SSH keys
* Sets up workspace
* Installs Java
* Installs Python
* Sets up virtualenv
* Installs [nickolashkraus/bash-scripts][nickolashkraus/bash-scripts]
* Installs [nickolashkraus/dotfiles][nickolashkraus/dotfiles]
* Installs Vim plugins
* Installs fzf
* Installs yabai
* Installs skhd
* Configures GUI applications

In a few weeks, I am starting a new job, so I can't wait to use this on a new
machine and to start being productive from day one.

[nickolashkraus/dotfiles]: https://github.com/nickolashkraus/dotfiles
[nickolashkraus/bash-scripts]: https://github.com/nickolashkraus/bash-scripts
[nickolashkraus/nhk-mac]: https://github.com/nickolashkraus/nhk-mac

[^1]: Writing good code, shipping products, and solving problems is also
important, but a far second[^1a].
[^1a]: This is satire.
[^2]: It's just some Bash.
