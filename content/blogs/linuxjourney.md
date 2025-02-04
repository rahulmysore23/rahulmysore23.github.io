---
title: "My Linux Journey: From Ubuntu to Hyprland, Vim to Tiling Window Managers, and Everything In Between"
date: 2025-01-30T16:12:25-06:00
draft: false
categories: ["setup"]
tags: ["tech", "linux", "window-managers", "i3", "hyprland", "vim", "neovim"]
---

If you’ve ever fallen down the rabbit hole of Linux, you know how addictive it can be. For me, it all started back in 2016. I was a curious kid with a laptop that barely ran Windows, and I stumbled upon Ubuntu. Little did I know, that first installation would spark a lifelong passion for tinkering, customizing, and optimizing my workflow.  

## The Beginning: Ubuntu and the Linux Curiosity  

I started with Ubuntu—like most people do. It was simple, user-friendly, and felt like a breath of fresh air compared to Windows. But soon, I wanted more. I tried Pop!_OS for its sleek design and gaming optimizations, Linux Mint for its familiarity, Fedora for its cutting-edge features, and even Garuda Linux because, let’s be honest, it looked *cool*.  

Back then, I was dual-booting with Windows, always keeping one foot in the familiar while dipping my toes into the Linux world. But as I spent more time in Linux, I realized how much more control and freedom it gave me. I was hooked.  

## The Vim Obsession  

Around the same time, I started hearing about Vim. One of my professors from 12th grade (back in 2014) had mentioned how his cousin was a Vim user and an incredible coder. He spoke about Vim like it was some kind of superpower. Naturally, I was intrigued.  

I decided to give Vim a shot. At first, it was frustrating. The modal editing, the commands, the steep learning curve—it felt like I was learning a new language. But once I got the hang of it, there was no going back. Vim became my go-to editor, and I started customizing it to fit my workflow.  

## The Tiling Window Manager Revelation  

One fine day, while browsing Twitch, I stumbled upon Linux streamers. These folks weren’t just coding or gaming—they were showcasing their setups. And boy, were they impressive. They were using something called *tiling window managers*.  

I had no idea what that meant, but it looked magical. No wasted screen space, no dragging windows around—just pure efficiency. I was determined to try it out.  

### My First Attempt: BSPWM  

I decided to start with BSPWM. I installed it, fired it up, and… I had no idea what I was doing. The screen was blank, and I couldn’t even open a terminal. I didn’t read the documentation (rookie mistake), and I quickly gave up, thinking I had messed up my system.  

## Back to Vim  

After the BSPWM disaster, I decided to stick with Vim for a while. I set up Neovim, which felt like a modern upgrade to Vim, and started configuring it for my Go development environment. It was amazing. I felt like a productivity wizard, zipping through code with my custom keybindings and plugins.  

But the allure of tiling window managers never left me. I kept watching those Twitch streamers, marveling at their setups, and dreaming of the day I could replicate it.  

## The i3 Era  

A few months later, I decided to give tiling window managers another shot. This time, I went with i3. Unlike BSPWM, i3 came with default configurations, which made it much easier to get started. I could open a terminal, resize windows, and even switch workspaces without pulling my hair out.  

I spent hours customizing my i3 setup, tweaking the config file, and adding keybindings. It felt like I was building my own operating system. I was in love.  

## Regolith Linux: The Game-Changer  

But as much as I enjoyed i3, I got tired of constantly tweaking and configuring. That’s when I discovered Regolith Linux—a pre-configured Ubuntu distro with i3 as the default window manager. It was perfect.  

I stuck with Regolith for *three years*. During that time, I became proficient with i3 and Vim. My workflow was smooth, and my setup was a thing of beauty. People would watch me code or work, amazed at how fast I could navigate through windows and edit files.  

## The Suckless Philosophy: DWM and ST  

While I was happy with i3, I couldn’t resist exploring other options. That’s when I came across DWM (Dynamic Window Manager) and the *suckless philosophy*.  

For those unfamiliar, suckless is all about simplicity, minimalism, and efficiency. Their software is lightweight, fast, and highly customizable—but only if you’re willing to dive into the source code.  

I decided to give DWM a try, along with ST (Simple Terminal). It was a steep learning curve, especially since I had to edit the config.h file and recompile everything. But it was worth it. DWM felt like i3 on steroids, and ST was the perfect companion.  

What really drew me to the suckless philosophy were these three hilarious and eye-opening websites:  

- [Motherfucking Website](https://motherfuckingwebsite.com/)  
- [Better Motherfucking Website](http://bettermotherfuckingwebsite.com/)  
- [Even Better Motherfucking Website](http://bettermotherfuckingwebsite.com/)  

These websites made me laugh out loud while also making me think about how bloated and overcomplicated modern web design can be. They perfectly encapsulate the suckless philosophy: keep it simple, stupid. Reading these sites inspired me to explore DWM and embrace minimalism in my own setup.  

## The Hyprland Experiment  

As much as I loved i3 and DWM, they lacked one thing: eye candy. Don’t get me wrong, they were incredibly efficient, but they felt like old tech. That’s when I discovered Wayland and Hyprland.  

### What is Wayland and Hyprland?  

Wayland is a modern display server protocol designed to replace the aging X11 system. It offers better performance, security, and simplicity. Hyprland, on the other hand, is a dynamic tiling window manager built for Wayland. It combines the efficiency of tiling window managers with modern aesthetics like smooth animations, rounded corners, and transparency.  

Hyprland is highly customizable and designed for users who want both functionality and beauty. It’s perfect for developers, designers, or anyone who spends a lot of time in their desktop environment.  

I tried Hyprland on Fedora during its early days, but it was still in development and didn’t work well for me. Frustrated, I switched back to i3.  

## The Present: Hyprland and Fedora  

Fast forward to today. After a six-month break from Linux (life happens), I’m back—and this time, I’m all in on Hyprland.  

I installed Fedora and used the [my-linux-for-work dotfiles](https://github.com/mylinuxforwork/dotfiles) to set up Hyprland. The setup process was seamless, thanks to a simple installation script provided by the creator. I was stunned by how polished and functional everything was.  

Hyprland is everything I’ve ever wanted in a window manager. It’s fast, efficient, and visually stunning. The animations are smooth, the customization options are endless, and it just *feels* right.  

I’ve made a few tweaks to the dotfiles to suit my preferences, but overall, it’s been a fantastic experience.  

![Hyprland terminal](/images/hyprland_terminals.jpg)
**Hyperland terminals with htop, cmatrix**

---

![Hyprland](/images/hyprland_vim_browser.jpg)
**Hyperland with browser, neovim**

---

## What’s Next?  

This is just the beginning. I’ve got a whole list of terminal tools and setups that I use to speed up my development workflow. I’ll be sharing those in another blog post soon.  

For now, I’m just enjoying the perfect blend of efficiency and aesthetics that Hyprland provides. If you’re curious, check out [Hyprland](https://hyprland.org/), [Regolith Linux](https://regolith-desktop.com/) and [Neovim](https://neovim.io/) to see what all the fuss is about.  

And if you’re just starting your Linux journey, don’t be afraid to experiment. It’s a wild ride, but it’s worth it.  

Until next time, happy tinkering!  

---  
