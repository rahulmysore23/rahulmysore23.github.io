---
title: "Fixing Audio Static and Hiss on Headless Debian"
date: 2026-04-12T00:00:00-05:00
draft: false
categories: ["linux"]
tags: ["tech", "linux", "debian", "audio", "alsa", "pulseaudio"]
---

I recently noticed some annoying static and hissing sounds coming from my speakers when the system was idle. It would stop when audio was playing, but come right back after. After some digging, I found the culprit — Debian's aggressive power saving on the audio card.

---

## The Symptom

You have speakers or headphones connected to your Debian machine (headless or not), and when the system is idle, you hear a low static or hissing sound. As soon as something plays audio, it goes quiet, then comes back a few seconds after the audio stops.

---

## The Fix

Open a terminal and check the current power save value:

```bash
cat /sys/module/snd_hda_intel/parameters/power_save
```

If it returns something like `10` (or any number greater than 0), that's your problem. The audio card is going to sleep after that many seconds of inactivity, and the wake-up process causes that static/hiss.

To fix it immediately (no reboot needed):

```bash
echo 0 | sudo tee /sys/module/snd_hda_intel/parameters/power_save
```

To make it permanent across reboots:

```bash
echo "options snd_hda_intel power_save=0" | sudo tee /etc/modprobe.d/audio_disable_powersave.conf
```

Verify the change:

```bash
cat /sys/module/snd_hda_intel/parameters/power_save
```

Should now return `0`.

---

## What If That Doesn't Work?

If you're using PulseAudio, it has its own suspend-on-idle behavior. Try disabling that:

```bash
sudo nano /etc/pulse/default.pa
```

Find this line:

```
load-module module-suspend-on-idle
```

And comment it out:

```
# load-module module-suspend-on-idle
```

Then restart PulseAudio:

```bash
pulseaudio -k
```

Or if you're on PipeWire (newer Debian versions), check its status:

```bash
systemctl --user status pipewire
```

---

That's it. One setting, and the hiss is gone. Happy listening!

