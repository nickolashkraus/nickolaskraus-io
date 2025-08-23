+++
title = 'My Local Development Setup (2025)'
date = 2025-06-03T00:00:00-00:00
lastmod = 2025-06-03T00:00:00-00:00
draft = false
description = '''
A comprehensive list of the hardware, software, and productivity modifications
that I use.
'''
tags = ['programming']
+++

It's been a while since I've provided an update on my local development setup.
*A lot* has changed in the intervening 7 years.

The following provides a comprehensive list of the hardware, software, and
productivity modifications that I use.

## Battlestation

My battlestation is where I do most of my work: programming, writing, admin,
etc. I'll be honest, I'm pretty proud of it.

- **Workstation**: [MacBook Pro (16-inch, 2021)][MacBook Pro (16-inch, 2021)][^1]
- **Monitors**: [Dell UltraSharp 27 4K][Dell UltraSharp 27 4K] (2)[^2]
- **Lighting**: [BenQ ScreenBar Halo LED][BenQ ScreenBar Halo LED] (2)
- **Keyboard**: [HHKB Professional HYBRID Type-S][HHKB Professional HYBRID Type-S]
- **Mouse**: [Logitech MX Master 3 Advanced][Logitech MX Master 3 Advanced][^3]
- **KVM Switch**: [AV Access iDock C20 USB-C][AV Access iDock C20 USB-C]
- **Webcam**: [Dell UltraSharp Webcam 4K UHD][Dell UltraSharp Webcam 4K UHD]
- **Desk**: [UPLIFT Standing Desk with Black Eco Curve Desktop (60"×30")][UPLIFT Standing Desk]
- **Chair**: [Steelcase Gesture][Steelcase Gesture]
- **Additional**:
  - [UPLIFT Range Single Monitor Arm][UPLIFT Range Single Monitor Arm] (2)
  - [UPLIFT Laptop Mount][UPLIFT Laptop Mount]

## Tactical Sidearm

When I'm on the go or traveling, I take my [MacBook Pro (14-inch, M4,
2024)][MacBook Pro (14-inch, M4, 2024)][^4].

## macOS

The setup for both my MacBooks is done via
[nickolashkraus/nhk-mac][nickolashkraus/nhk-mac]. When setting up a new macOS
workstation, all I need to do is:

1. Open Terminal.
2. Download the zip file.

    ```bash
    curl -L https://github.com/nickolashkraus/nhk-mac/archive/master.zip \
      -o nhk-mac.zip
    unzip -q nhk-mac.zip
    mv nhk-mac-master nhk-mac
    rm nhk-mac.zip
    cd nhk-mac
    ```

3. Run `nhk-mac`.

    ```bash
    ./nhk-mac
    ```

**Rebind Caps Lock to Control**

To rebind Caps Lock (⇪) to Control (⌃) go to
**System Settings** > **Keyboard** > **Keyboard Shortcuts…**.
Under **Modifier Keys** change **Caps Lock (⇪) key** to **⌃Control**.

## Software

All configuration is done via
[nickolashkraus/dotfiles][nickolashkraus/dotfiles].

- **Terminal**: [iTerm2][iTerm2]
- **Terminal Multiplexer**: [tmux][tmux]
- **Shell**: [Zsh][Zsh] + [Oh My Zsh][Oh My Zsh]
- **Editor**: [Vim][Vim]
- **Window Manager**: [yabai][yabai]
- **Hotkeys**: [skhd][skhd]
- **Fuzzy Finder**: [fzf][fzf]
- **Borders**: [JankyBorders][JankyBorders]
- **Statusline**: [Powerline][Powerline]
- **Media Player**: [mpv][mpv]
- **App Launcher**: [Alfred][Alfred]
- **Kubernetes**: [k9s][k9s]
- **Package Manager**: [Homebrew][Homebrew]
- **AI Agents**: [Codex CLI][Codex CLI], [Claude Code][Claude Code], [Gemini CLI][Gemini CLI]
- **Browser**: [Firefox][Firefox]
- **Notetaking**: [Bear][Bear]
- **Calendar**: [Fantastical][Fantastical]
- **Todolist**: [Todoist][Todoist]

## God Mode

>For whosoever hath, to him shall be given, and he shall have more abundance:
>but whosoever hath not, from him shall be taken away even that he hath.

—*Matthew 13:12*

Since the latter half of the twentieth century, a class of technological
elites—the technocracy—has amassed significant wealth, power, and influence.
Their rise has been facilitated by advances in technology, which allow
individuals to think and act more quickly and at greater scale. This
technologically empowered class has been further augmented by the advent of
large language models, which enhance their productivity and effectiveness. If
this trend continues, the world will become increasingly dominated by those who
can harness the asymmetrical advantages afforded by technology. For this
reason, I've invested an inordinate amount of time tuning and optimizing my
development setup and workflows to better leverage both technology and AI.

Here is a rundown of my development setup and workflows:
- Two displays: left is vertical, right is horizontal.
  - Left: Full-screen terminal. This window is used for text editing,
    interacting with the shell, etc.
  - Right: Full-screen terminal. This window is used for my AI agents
    (Codex CLI, Claude Code, Gemini CLI). Additionally, a layer (via yabai)
    comprises my applications (Firefox, Bear, Slack, etc.).[^5]
- Hotkeys are configured for quickly switching between terminal windows
  (`Ctrl - Return`) or each terminal individually (`Ctrl - 1` and `Ctrl - 2`).
  To summon the browser, I use `Ctrl - 3`.
- Using tmux, I can scroll, select, copy, and paste text. In the browser, I use
  [Vimium][Vimium] for keyboard-driven navigation.[^6]

[^1]: Apple M1 Max, 64 GB Memory
[^2]: Left monitor is vertical, right monitor is horizontal.
[^3]: Logitech released an update to the Logitech MX Master 3 Advanced, the
Logitech MX Master 3S, in May 2022. They no longer offer the Master 3 Advanced
on their website.
[^4]: Apple M4 Max, 32 GB Memory
[^5]: I've found that since I started using AI agents more heavily, my reliance
on the browser has decreased.
[^6]: Using this workflow, I rarely have to use my mouse.

[MacBook Pro (16-inch, 2021)]: https://support.apple.com/en-us/111901
[Dell UltraSharp 27 4K]: https://www.dell.com/en-us/shop/dell-ultrasharp-27-4k-usb-c-hub-monitor-u2723qe/apd/210-bdpf/computer-monitors
[BenQ ScreenBar Halo LED]: https://www.benq.com/en-us/lighting/monitor-light/screenbar-halo.html
[HHKB Professional HYBRID Type-S]: https://hhkeyboard.us/hhkb/pro-hybrid-type-s
[AV Access iDock C20 USB-C]: https://www.avaccess.com/products/idock-c20-kvm-switch-docking-station
[Logitech MX Master 3 Advanced]: https://www.logitech.com/en-us/shop/p/mx-master-3s
[Dell UltraSharp Webcam 4K UHD]: https://www.dell.com/en-us/shop/dell-ultrasharp-webcam-wb7022-4k-uhd/apd/319-bbhp/pc-accessories
[UPLIFT Standing Desk]: https://www.upliftdesk.com/uplift-v2-standing-desk-v2-or-v2-commercial
[Steelcase Gesture]: https://www.steelcase.com/products/office-chairs/gesture
[UPLIFT Range Single Monitor Arm]: https://www.upliftdesk.com/range-single-monitor-arm
[UPLIFT Laptop Mount]: https://www.upliftdesk.com/laptop-mount-by-uplift-desk
[MacBook Pro (14-inch, M4, 2024)]: https://support.apple.com/en-us/121552
[nickolashkraus/nhk-mac]: https://github.com/nickolashkraus/nhk-mac
[nickolashkraus/dotfiles]: https://github.com/nickolashkraus/dotfiles
[iTerm2]: https://iterm2.com
[tmux]: https://github.com/tmux/tmux
[Zsh]: https://www.zsh.org
[Oh My Zsh]: https://ohmyz.sh
[Vim]: https://www.vim.org
[yabai]: https://github.com/koekeishiya/yabai
[skhd]: https://github.com/koekeishiya/skhd
[fzf]: https://junegunn.github.io/fzf
[JankyBorders]: https://github.com/FelixKratz/JankyBorders
[Powerline]: https://powerline.readthedocs.io/en/master/index.html
[mpv]: https://mpv.io
[Alfred]: https://www.alfredapp.com
[k9s]: https://k9scli.io
[Homebrew]: https://brew.sh
[Codex CLI]: https://github.com/openai/codex
[Claude Code]: https://github.com/anthropics/claude-code
[Gemini CLI]: https://github.com/google-gemini/gemini-cli
[Firefox]: https://www.mozilla.org/en-US/firefox/new/
[Bear]: https://bear.app
[Fantastical]: https://flexibits.com/fantastical
[Todoist]: https://www.todoist.com
[Vimium]: https://vimium.github.io
