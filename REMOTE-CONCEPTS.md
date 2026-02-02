# Remote OpenClaw Deployment: Concepts & Architecture

This document explains the "why" behind running OpenClaw on a remote server instead of your local machine.

---

## Why Run OpenClaw Remotely?

### The Problem with Local Installation

OpenClaw is powerful—it can read files, execute commands, browse the web, and interact with your system. Running it directly on your personal computer means:

- **Security risk**: An AI agent has access to your personal files, passwords, and system
- **Always-on requirement**: Your laptop needs to stay running for the bot to work
- **Resource usage**: AI processing competes with your daily work

### The Solution: Isolation

By running OpenClaw on a separate server (whether a cloud VPS or a local virtual machine), you get:

- **Sandboxed environment**: The agent can only access what's on that server
- **24/7 availability**: The server runs independently of your laptop
- **Clean separation**: Your personal data stays on your machine, the bot lives elsewhere

---

## Why We Need a VPN (Tailscale)

### The Security Challenge

When you put OpenClaw on a remote server, you need to connect to it somehow. The traditional options are problematic:

- **Public IP + open ports**: Anyone on the internet could try to access your server
- **Complex firewall rules**: Easy to misconfigure and create security holes
- **VPN servers**: Complicated to set up, certificates to manage

### What Tailscale Does

Tailscale creates a private network between your devices using WireGuard encryption. Think of it as a secure tunnel that only your devices can use.

**Key benefits:**

- **Zero configuration**: Install it, log in, done
- **No open ports**: Your server stays invisible to the public internet
- **Works anywhere**: Connects through firewalls, NAT, coffee shop WiFi
- **Private IP addresses**: Each device gets a 100.x.x.x address only visible to your other devices

---

## Architecture Overview

```
┌─────────────────┐         Tailscale Mesh (WireGuard)         ┌─────────────────┐
│   Your Mac      │◄─────────────────────────────────────────►│  DigitalOcean   │
│  (local dev)    │            100.x.y.z ◄─► 100.x.y.z        │    Droplet      │
│                 │                                            │                 │
│ /.openclaw      │         File Transfer: Taildrop            │ OpenClaw        │
│ (existing)      │◄─────────────────────────────────────────►│  Gateway        │
└─────────────────┘              or rsync                      └─────────────────┘
```

**How it works:**

1. Your Mac and the server both run Tailscale
2. They automatically find each other and establish an encrypted connection
3. You access OpenClaw through this private tunnel
4. No one else on the internet can see or reach your server

---

## File Transfer Options

You'll often need to move files between your Mac and the server—configuration files, documents for the AI to process, outputs to review.

```
┌──────────────────────────┬────────────────────────┬──────────────────────────────────────────────────────┐
│          Method          │        Best For        │                     How it Works                     │
├──────────────────────────┼────────────────────────┼──────────────────────────────────────────────────────┤
│ Taildrop                 │ Quick file transfers   │ Built into Tailscale, encrypted P2P, supports resume │
├──────────────────────────┼────────────────────────┼──────────────────────────────────────────────────────┤
│ Taildrive                │ Persistent file access │ WebDAV folders accessible across your tailnet        │
├──────────────────────────┼────────────────────────┼──────────────────────────────────────────────────────┤
│ rsync over Tailscale SSH │ Syncing directories    │ Traditional rsync using Tailscale's secure tunnel    │
└──────────────────────────┴────────────────────────┴──────────────────────────────────────────────────────┘
```

**Taildrop** is the easiest—drag and drop files between devices, just like AirDrop but across the internet.

**Taildrive** is better when you need ongoing access to folders, like mounting a remote drive.

**rsync** is for developers who want to sync entire project directories.

---

## Why DigitalOcean?

There are many cloud providers. DigitalOcean stands out for this use case because:

- **1-Click OpenClaw deployment**: Pre-configured, security-hardened image available
- **Simple pricing**: $12-24/month, no surprise bills
- **Good documentation**: Beginner-friendly guides
- **Reasonable specs**: 2GB RAM is enough for OpenClaw

Other options like Hetzner offer better price-per-performance (especially in Europe), but DigitalOcean's ease of use makes it ideal for getting started.

---

## The Two Deployment Paths

### Path 1: Local VM (OrbStack)

**Best for:** Learning, demos, development, testing

Run a Linux virtual machine on your Mac using OrbStack. This gives you the isolation benefits without cloud costs. Your "server" is just a lightweight VM on your laptop.

**Trade-off:** Only works when your Mac is running.

### Path 2: Cloud VPS (DigitalOcean)

**Best for:** Always-on assistant, production use, accessing from multiple devices

Run OpenClaw on a real cloud server. Access it from anywhere—your laptop, phone, or tablet.

**Trade-off:** Monthly hosting cost ($12-24) plus AI API costs.

---

## Summary

| Component | Purpose |
|-----------|---------|
| **OpenClaw** | The AI assistant that does the work |
| **VPS/VM** | Isolated environment where it runs safely |
| **Tailscale** | Secure private network to access it |
| **Taildrop** | Easy file transfer between your devices |

The goal is simple: run a powerful AI assistant that's always available, without exposing your personal computer or data to risk.

---

## References

- [OpenClaw](https://openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [DigitalOcean OpenClaw Tutorial](https://www.digitalocean.com/community/tutorials/how-to-run-openclaw)
- [DigitalOcean 1-Click Deploy](https://marketplace.digitalocean.com/apps/openclaw)
- [Tailscale Quick Start](https://tailscale.com/kb/1017/install)
- [Taildrop Documentation](https://tailscale.com/kb/1106/taildrop)
- [dabit3's Setup Gist](https://gist.github.com/dabit3/42cce744beaa6a0d47d6a6783e443636)
