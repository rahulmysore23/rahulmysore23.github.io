---
title: "My Home Lab Setup"
date: 2026-04-08T00:00:00-05:00
draft: false
categories: ["home-lab"]
tags: ["tech", "linux", "homelab", "k8s", "docker", "debian"]
---

I finally decided to set up a home lab. After some research, I went with an **HP EliteDesk 800 G6** with these specs:

- **CPU**: Intel Core i5 10th Gen
- **RAM**: 16 GB
- **Storage**: 256 GB NVMe

I installed **Debian 13 (Trixie)** without a desktop environment a headless server all the way.

---

## Installing Docker Engine

Since we're running headless (no GUI), we install **Docker Engine**, not Docker Desktop (which requires a desktop environment).

```bash
# Remove old versions if any
sudo apt remove docker docker.io containerd runc

# Install prerequisites
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker's GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg \
  -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/debian \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin

# Add your user to docker group (no sudo needed)
sudo usermod -aG docker $USER

# Enable and start Docker
sudo systemctl enable docker
sudo systemctl start docker

# Verify
docker run --rm hello-world
```

> **Note**: Log out and back in for group changes to take effect, or run `newgrp docker`.

---

## Server Hardening

For basic security, I installed **fail2ban** to protect against brute force attacks:

```bash
sudo apt install fail2ban -y
sudo systemctl enable fail2ban --now
```

Then I set up **UFW** (Uncomplicated Firewall):

```bash
sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable
```

It'll warn you that enabling UFW may disrupt SSH—type `y` and hit enter.

Verify both are running:

```bash
sudo ufw status
sudo systemctl status fail2ban
```

UFW should show `Status: active` with a rule allowing SSH.

---

## Tailscale VPN

I installed **Tailscale** on my laptop, phone, and server for easy remote access.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Your nodes will show up in the Tailscale admin console. You can then SSH into your server from anywhere using the Tailscale IP or MagicDNS name (e.g., `user@your-server.tailnet.ts.net`). I used MagicDNS to access Portainer from my laptop and it worked perfectly.

---

## Portainer

For a web UI to manage containers, I installed **Portainer**:

```bash
docker volume create portainer_data

docker run -d \
  --name portainer \
  --restart unless-stopped \
  -p 9000:9000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

Access it at `http://your-tailscale-server:9000`.

---

## Pi-hole DNS Sinkhole

**Why?** Pi-hole blocks ads and trackers at the network level by acting as a DNS sinkhole. Instead of installing ad blockers on every device, you just point your DNS to Pi-hole and it blocks requests to known ad/tracker domains.

### Deploy via Portainer

Go to **Stacks → Add stack**, name it `pihole`, and paste:

```yaml
services:
  pihole:
    image: pihole/pihole:latest
    container_name: pihole
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80"
    environment:
      - WEBPASSWORD=changeme    # change this!
      - TZ=America/New_York
    volumes:
      - ./etc-pihole:/etc/pihole
      - ./etc-dnsmasq.d:/etc/dnsmasq.d
    restart: unless-stopped
```

Hit **Deploy the stack**.

### Configure Tailscale DNS

**Why?** Instead of configuring DNS on each device or router, Tailscale lets you push DNS settings to all connected devices. All your devices will automatically use Pi-hole for DNS queries.

Go to **tailscale.com → Admin Console → DNS**:

1. Scroll to **Nameservers → Add nameserver → Custom**
2. Enter your server's Tailscale IP (100.x.x.x)
3. Check **Override local DNS**
4. Hit **Save**

All Tailscale devices will now route DNS through Pi-hole.

### Fix Password Issue (If Needed)

If the WEBPASSWORD env var didn't set properly (known Pi-hole issue), reset it:

```bash
docker exec -it pihole pihole setpassword yourpassword
```

Access Pi-hole admin at `http://your-server/admin`. Check the **Query Log**
to see traffic flowing.

---

## What's Next?

This is just the beginning. Future posts will cover:

- Installing k3s (lightweight k8s for homelabs)
- Setting up networking and DNS
- Deploying my first workloads
- Monitoring and observability

Stay tuned!
