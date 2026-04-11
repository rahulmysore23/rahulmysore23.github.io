---
title: "My Home Lab Setup - Part 2"
date: 2026-04-10T00:00:00-05:00
draft: true
categories: ["home-lab"]
tags: ["tech", "linux", "homelab", "docker", "portainer", "n8n", "uptime-kuma"]
---

In [Part 1](/blogs/my-home-lab-setup/), we set up Debian, Docker, fail2ban, UFW,  Tailscale, Portainer, and Pi-hole. Now let's add some actual workloads.

---

## Deploying Uptime Kuma + n8n via Portainer

Since we already have Portainer running from Part 1, let's deploy our next two services using **Stacks**. This gives us a clean way to manage multi-container setups.

### The Stack

Go to **Stacks → Add stack**, name it `home-lab-services`, and paste:

```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    ports:
      - "3001:3001"
    volumes:
      - ./uptime-kuma-data:/app/data
    restart: unless-stopped

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=0.0.0.0
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - WEBHOOK_URL=http://your-tailscale-ip:5678/
      - GENERIC_TIMEZONE=America/New_York
      - N8N_ENCRYPTION_KEY=change-this-to-a-long-random-string
      - N8N_DIAGNOSTICS_ENABLED=false
      - N8N_SECURE_COOKIE=false
    volumes:
      - ./n8n-data:/home/node/.n8n
    restart: unless-stopped
```

Hit **Deploy the stack**.

### Troubleshooting n8n Permission Issues

If n8n fails to start, it's likely a folder permission issue. The container runs as
user ID 1000, but the volume might be owned by root.

Check where your n8n data lives:

```bash
find / -name "n8n-data" -type d 2>/dev/null
```

Fix the ownership:

```bash
sudo chown -R 1000:1000 /data/compose/2/n8n-data
```

Restart the container in Portainer and it should come up clean.

### Fixing n8n Secure Cookie Error

If you see an error about secure cookies in Safari (or get redirected to HTTP),
add these env vars:

```yaml
environment:
  - N8N_DIAGNOSTICS_ENABLED=false
  - N8N_SECURE_COOKIE=false
```

Redeploy the stack and n8n should load without issues.

---

## What Do These Do?

**Uptime Kuma** is a self-hosted monitoring tool. Think of it like a simpler,
open-source version of Pingdom. You add your services ( websites, APIs, docker
containers) and it checks them at intervals. Gets notified via email, Telegram,
Discord, etc. when something goes down.

Access it at `http://your-tailscale-ip:3001`.

**n8n** is a workflow automation tool. It's like Zapier, but self-hosted and open
source. You can connect APIs, schedule tasks, move data between services, and
automate all kinds of things without writing code.

Access it at `http://your-tailscale-ip:5678`.

---

## What's Next?

These two are just the start. Future posts will cover:

- Setting up n8n workflows for real automation
- Adding monitoring for our Docker containers

Stay tuned!
