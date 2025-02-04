---
title: "Building a TUI for Managing Systemd Services: A Fun Side Project"
date: 2025-02-04T01:06:28-06:00
draft: false
categories: ["side-project"]
tags: ["tech", "linux", "Go", "systemd", "TUI"]
---

A few months ago, I found myself constantly juggling multiple custom services at work. These services were binaries written in Go, designed for data observability purposes. While they were efficient at their job, managing them was a bit of a hassle. I often had to check their status, restart them, or stop them entirely using the `systemctl` command. Typing `systemctl status <service>` over and over again started to feel tedious. Sure, I could have created aliases, but I wanted something more interactive—a **TUI (Terminal User Interface)** that would make the process smoother and more enjoyable.

That’s when I decided to start a side project: **sysdmon**, a TUI-based systemd service manager written in Go. Like many personal projects, it sat on the shelf for a while. But recently, I dusted it off, spent a couple of hours polishing it, and now it’s ready to share with the world.

---

## Why I Built sysdmon

At work, I often had to manage multiple custom services. These services were critical for monitoring and analyzing data, but managing them manually using `systemctl` was time-consuming. I wanted a tool that could:

1. **List all systemd services** in one place.
2. **Show detailed information** about each service (status, memory usage, CPU usage, etc.).
3. Allow me to **start, stop, and restart services** with simple keybindings.
4. **Filter services** by name for quick access.

While aliases or scripts could have solved some of these problems, I wanted a more interactive and visually appealing solution. That’s how **sysdmon** was born.

---

![sysdmon](/images/sysdmon.jpg)

## What is sysdmon?

---

**sysdmon** is a lightweight, TUI-based tool for managing systemd services. It’s written in Go and uses the [tview](https://github.com/rivo/tview) library for the terminal interface. Here’s what it can do:

### Features

- **List all systemd services**: Scroll through a list of all services on your system.
- **View service details**: Get detailed information about a service, including:
  - **Status**: Active, inactive, or failed.
  - **Memory Usage**: Human-readable memory consumption (e.g., 512 MB).
  - **CPU Usage**: CPU usage as a percentage.
  - **Enabled State**: Whether the service is enabled or disabled.
- **Filter services**: Search for services by name in real-time.
- **Control services**: Start, stop, or restart services directly from the TUI.
- **Custom service list**: Load a custom list of services from a JSON file.

### Keybindings

- **Ctrl+S**: Start the selected service.
- **Ctrl+T**: Stop the selected service.
- **Ctrl+R**: Restart the selected service.
- **Ctrl+F**: Focus on the search bar.
- **Enter**: Return focus to the services list.

---

## How to Use sysdmon

### Installation

You can install **sysdmon** using `go install`:

```bash
go install github.com/rahulmysore23/sysdmon@latest
```

Alternatively, you can download pre-built binaries from the [Releases](https://github.com/rahulmysore23/sysdmon/releases) page.

### Running sysdmon

#### Default Mode (List All Services)

To list all systemd services, simply run:

```bash
sysdmon
```

#### Custom Mode (Load Services from JSON File)

If you want to load a custom list of services, create a JSON file (`config.json`) like this:

```json
{
  "services": [
    "nginx.service",
    "docker.service",
    "ssh.service"
  ]
}
```

Then run:

```bash
sysdmon -config /path/to/config.json
```

---

## The Journey of Building sysdmon

Building **sysdmon** was a fun and rewarding experience. It started as a simple idea to make my workflow more efficient, but it quickly turned into a learning opportunity. I got to explore Go’s `dbus` package for interacting with systemd, and I discovered the power of TUI libraries like `tview`.

Of course, like most personal projects, it sat untouched for a while. But when I finally revisited it, I was able to polish it up and add some nice features, like filtering and custom service lists. It’s amazing what you can accomplish in just a couple of hours when you’re motivated!

---

## Future Improvements

While **sysdmon** is functional, there’s always room for improvement. Here are some ideas for future enhancements:

1. **Service Logs Integration**: View real-time logs for a selected service.
2. **Bulk Operations**: Start, stop, or restart multiple services at once.
3. **Remote System Management**: Manage services on remote systems via SSH.
4. **Custom Themes**: Add support for dark mode and custom colors.
5. **Plugin System**: Allow users to extend functionality with plugins.

If you’re interested in contributing, feel free to check out the [GitHub repository](https://github.com/rahulmysore23/sysdmon). Pull requests and ideas are always welcome!

---

## Final Thoughts

Building **sysdmon** was a fun and fulfilling side project. It not only solved a personal pain point but also gave me a chance to dive deeper into Go and TUI development. If you’re someone who manages systemd services frequently, I hope you find this tool useful. And if you have any feedback or ideas for improvement, don’t hesitate to reach out or contribute to the project.

Happy coding! 🚀
