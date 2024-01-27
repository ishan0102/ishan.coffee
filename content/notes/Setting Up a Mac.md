---
title: Setting Up a Mac
date: 2023-12-02
tags:
  - seed
enableToc: false
---
## System Preferences
- Trackpad: disable force touch, make tracking speed fast, disable natural scrolling
- Keyboard: key repeat rate fast, delay until repeat short
- Keyboard > Shortcuts > Screenshots: change the shortcut for "copy picture of selected area to the clipboard" to `⌘\` for ease (it's a game changer trust me)
- Displays: disable automatically adjust brightness, disable true tone
- Desktop > Hot Corners: mission control, notification center, launchpad, put display to sleep
- Enable screen sharing permissions for Zoom
## Apps
- Remove everything from the dock
- Install VS Code, Sublime Text, Chrome, Spotify, Obsidian, Notion, Zoom
- Nice to haves: Klack, Vivid, Cron, Tailscale, Texts, Stats, Screen Studio, Rectangle, Warp, Raycast, Cyberduck, Heynote, Ollama, LM Studio

## Terminal
- Install command line tools: `xcode-select --install`
- Don't show login info: `touch .hushlogin`
- Change zsh to bash: `chsh -s /bin/bash`
- Get custom dotfiles: `git clone https://github.com/ishan0102/dotfiles.git`
- Add SSH key for GitHub
- Disable visual bell