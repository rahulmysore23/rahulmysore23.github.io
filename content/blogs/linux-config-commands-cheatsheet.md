---
title: "Linux Config Commands Cheat Sheet"
date: 2026-03-21T12:00:00-04:00
draft: true
---

## Kitty Terminal

### Fix Kitty Theme Not Applying

The issue is often pywal escape codes overriding your theme.

```bash
# Comment out pywal in your zshrc autostart
# ~/.config/zshrc/30-autostart
# cat ~/.cache/wal/sequences

# Reload kitty config
killall -USR1 kitty
```

### Launch Kitty with Specific Config

```bash
kitty --config ~/.config/kitty/kitty.conf &
```

## Desktop Environment

### Set Default Browser

```bash
# Check current default
xdg-settings get default-web-browser

# Set new default (replace with your browser's .desktop file)
xdg-settings set default-web-browser userapp-Zen-UQOSA3.desktop

# For Chrome
xdg-settings set default-web-browser google-chrome.desktop

# For Firefox
xdg-settings set default-web-browser firefox.desktop

# For Flatpak apps, find the desktop file first
flatpak list | grep -i <browser-name>
```

### Common xdg-settings Commands

```bash
# View all available settings
xdg-settings --list

# Get/Set default browser
xdg-settings get default-web-browser
xdg-settings set default-web-browser <desktop-file>

# Get/Set default mail client
xdg-settings get default-mail-client
xdg-settings set default-mail-client <desktop-file>

# Get/Set file manager
xdg-settings get default-file-manager
xdg-settings set default-file-manager <desktop-file>
```

## Finding Desktop Files

```bash
# Search for installed apps
find ~/.local/share/applications -name "*.desktop" | grep -i <app>
find /usr/share/applications -name "*.desktop" | grep -i <app>

# For Flatpak apps
ls ~/.var/app/*/data/applications/

# Check MIME types
xdg-mime query default text/html
```

## Process Management

```bash
# Kill and restart a process
killall -USR1 kitty    # Reload config (graceful)
killall kitty           # Force kill
killall -9 kitty       # Force kill (ungraceful)
```

## Quick Reference

| Task | Command |
|------|---------|
| Reload kitty config | `killall -USR1 kitty` |
| Find desktop file | `flatpak list \| grep -i <app>` |
| Set default app | `xdg-settings set <setting> <file.desktop>` |
| Check default app | `xdg-settings get <setting>` |
