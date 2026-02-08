---
title: "Neovim AI Tools"
date: 2026-02-03T22:31:23-05:00
draft: false
categories: ["setup"]
tags: ["tech", "linux", "AI", "nvim", "vim", "neovim", "windsurf", "opencode"]
---

Most developers today use AI tools to boost productivity and move faster while coding. New models and tools are being released almost every day, and we now have full-fledged AI-powered IDEs that try to handle everything for you.

Editors like [Cursor](https://cursor.sh/), [Windsurf](https://codeium.com/windsurf), [Kiro](https://kiro.ai/), and [Antigravity](https://antigravity.google/) are a few popular examples.

These IDEs are both **good and bad**.

They’re good because they genuinely improve productivity and reduce friction. But they’re also bad because it’s very easy to rely on them *too much* and slowly lose touch with what’s actually happening in your code.

That said, I really like these tools. I’ve gotten used to them and use them daily to improve my workflow—but not to the point where I feel stuck or helpless without them.

## The Cursor-like feel in Neovim

I’ve been looking for ways to recreate a Cursor-like experience inside Neovim. There *are* plugins that get you close, but nothing is a perfect one-to-one replacement.

I’ve tried quite a few. Some worked well, some didn’t, and some were more trouble than they were worth. After experimenting for a while, I’ve landed on a setup that lets me stick with Neovim while still getting most of the AI goodness I want.

## Currently using

* **[windsurf.nvim](https://github.com/Exafunction/windsurf.nvim)**
  I use this mainly for inline completions. This is the **official Windsurf plugin**, completely free, and very easy to set up. It does exactly what it promises and integrates nicely with Neovim.

* **[claudecode.nvim](https://github.com/coder/claudecode.nvim)**
  I use this because my current employer provides a Claude subscription. It opens a chat window inside Neovim, has solid keybindings, and overall feels very polished. In my experience, Claude is still one of the best models for coding tasks.

* **[opencode.nvim](https://github.com/nickjvandyke/opencode.nvi)**
  I use this as an alternative to Claude Code. It’s free and open source, comes with a free model, and also lets you configure API keys for other providers. Right now I’m experimenting with **Gemini**—it’s not amazing, but it’s cheap, and I don’t really want to use DeepSeek.

* **[wtf.nvim](https://github.com/piersolenski/wtf.nvim)**
  This one’s a bit experimental, but interesting. It uses Neovim diagnostics as input and provides AI-generated suggestions based on them. Still rough around the edges, but fun to play with and potentially very powerful.

## Tried before

* **[avante.nvim](https://github.com/yetone/avante.nvim)**
  This is probably the most starred AI plugin for Neovim and aims to fully emulate a Cursor-like workflow. I had some trouble setting it up with Gemini. I did manage to get it working, but eventually ran into infinite loop issues—most likely due to the model. I’ll probably revisit this if I switch to a different provider.

## More plugins

If you want to explore more options, this repository has a great curated list of Neovim AI plugins:

[https://github.com/ColinKennedy/neovim-ai-plugins](https://github.com/ColinKennedy/neovim-ai-plugins)

There are a lot of cool ideas there, even if you don’t end up using most of them.

## What’s next

I’ll cover the setup for these plugins in detail in the next post. The setup itself is pretty straightforward—I use [LazyVim](https://www.lazyvim.org/), and all of these plugins work well with it out of the box.

I’m also genuinely curious to hear what other plugins people are using and how they’ve integrated AI into their Neovim workflow. If you’ve found something interesting (or something that completely didn’t work), I’d love to hear about it.

Always looking for new ideas and better workflows.
