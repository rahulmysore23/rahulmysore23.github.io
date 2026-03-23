---
title: "Fixing Kitty Theme and Setting Default Browser"
date: 2026-03-21T12:00:00-04:00
draft: true
---

## The Problem

My kitty terminal wasn't applying the Catppuccin Mocha theme I had configured. The colors would stay on the old pywal-generated colors no matter what I did.

## Root Cause

The issue was `pywal` - a tool that generates colorschemes from your wallpaper. It was being run in my zshrc autostart file and sending escape codes that overrode kitty's colors:

```bash
# In ~/.config/zshrc/30-autostart
cat ~/.cache/wal/sequences
```

This command sends OSC (Operating System Command) escape codes that tell the terminal to use pywal's cached colors, ignoring any theme configured in kitty.conf.

## The Fix

Comment out the pywal line in your zshrc autostart:

```bash
# ~/.config/zshrc/30-autostart
# -----------------------------------------------------
# Pywal (disabled - using kitty theme)
# -----------------------------------------------------
# cat ~/.cache/wal/sequences
```

Then reload kitty with `killall -USR1 kitty` and your theme should apply.

## Bonus: Setting Default Browser

I also helped set Zen Browser as the default browser using xdg-settings:

```bash
# Find the desktop file
xdg-settings get default-web-browser

# Set your preferred browser
xdg-settings set default-web-browser userapp-Zen-UQOSA3.desktop
```

## Key Takeaways

1. If your terminal theme isn't applying, check if something (like pywal, wal, or a shell integration) is sending color escape codes
2. Escape codes in your shell config run every time you start a new shell
3. Tools like pywal can persist colors across sessions via cached sequences
