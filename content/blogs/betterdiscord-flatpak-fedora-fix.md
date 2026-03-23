---
title: "Fixing BetterDiscord with Discord Flatpak on Fedora"
date: 2026-03-22T12:00:00-04:00
draft: true
---

## The Problem

After installing BetterDiscord on Fedora, Discord wouldn't open at all. It would crash immediately on launch with an error about a missing `betterdiscord.asar` module.

## Root Cause

The issue stems from how Flatpak isolates applications. When Discord is installed via Flatpak:

1. BetterDiscord was installed at `~/.config/BetterDiscord/data/betterdiscord.asar`
2. But Flatpak Discord runs in an isolated sandbox and looks for it at `~/.var/app/com.discordapp.Discord/config/BetterDiscord/data/betterdiscord.asar`

These are different paths, so Discord couldn't find the BetterDiscord module.

## The Fix

Create a symlink to bridge the two paths:

```bash
mkdir -p ~/.var/app/com.discordapp.Discord/config/BetterDiscord/data
ln -sf ~/.config/BetterDiscord/data/betterdiscord.asar ~/.var/app/com.discordapp.Discord/config/BetterDiscord/data/betterdiscord.asar
```

Now launch Discord with:

```bash
flatpak run com.discordapp.Discord
```

BetterDiscord should load successfully.

## Installing Themes

BetterDiscord themes go in `~/.var/app/com.discordapp.Discord/config/BetterDiscord/themes/`. For example, to install a downloaded theme:

```bash
mv ~/Downloads/my-theme.css ~/.var/app/com.discordapp.Discord/config/BetterDiscord/themes/
```

After moving the file, enable it in Discord via BetterDiscord Settings > Themes.

## Key Takeaways

1. Flatpak apps run in isolated sandboxes with their own config directories
2. When installing mods/plugins for Flatpak apps, you may need to create symlinks to the sandboxed config paths
3. You can verify the correct path by checking the error logs - they'll show where the app is looking for the missing file
