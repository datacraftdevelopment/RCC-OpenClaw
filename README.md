# RCC OpenClaw Deployment Guide

Best practices and documentation for deploying [OpenClaw](https://openclaw.ai), the open-source personal AI assistant.

**New to OpenClaw?** Start with [What Is OpenClaw?](WHAT-IS-OPENCLAW.md) to understand what it does and why you might want it.

---

## Documents

| Document | Description |
|----------|-------------|
| [WHAT-IS-OPENCLAW.md](WHAT-IS-OPENCLAW.md) | What it is, why use it, how it compares to ChatGPT/Claude Code |
| [REMOTE-CONCEPTS.md](REMOTE-CONCEPTS.md) | Why run remotely, VPN basics, architecture overview |
| [LOCAL-NATIVE-SETUP.md](LOCAL-NATIVE-SETUP.md) | Install directly on your Mac |
| [LOCAL-VM-SETUP.md](LOCAL-VM-SETUP.md) | Run in a local VM using OrbStack |
| [HOSTED-SETUP.md](HOSTED-SETUP.md) | Deploy on DigitalOcean with Tailscale |

---

## Choosing a Setup

| Your Situation | Setup | Why |
|----------------|-------|-----|
| Desktop Mac, always on, want file access | [Local Native](LOCAL-NATIVE-SETUP.md) | Simplest. AI can work with your files directly. |
| Laptop, or want a safe sandbox | [Local VM](LOCAL-VM-SETUP.md) | Isolated but no cloud costs. Easy to reset. |
| Access from anywhere, multiple devices | [Cloud VPS](HOSTED-SETUP.md) | Always on, reach it from your phone. |

**Not sure?** Read [Remote Concepts](REMOTE-CONCEPTS.md) for more on deployment architecture.

---

## The Core Trade-off: Isolation vs. File Access

There's no single "right" way to run OpenClaw. It depends on how you want to use it.

### Isolation (VM or VPS)

**What you get:**
- AI agent can't access your personal files, passwords, or system
- Safe environment to experiment with prompts
- Easy to reset if something goes wrong

**What you lose:**
- Can't say "clean up my desktop" or "review that file I just downloaded"
- Need to manually transfer files to/from the isolated environment
- More infrastructure to manage

### Local Native (No Isolation)

**What you get:**
- AI can work directly with your files: organize, summarize, process
- Simpler setup, nothing extra to manage
- Natural workflow: "Hey, check my Downloads folder"

**What you lose:**
- AI has access to everything you have access to
- Need to trust the prompts you're running
- Harder to undo if something goes wrong

---

## Setup Details

### Local Native (No Isolation)

Best for desktop Macs that are always on—Mac Mini, Mac Studio, iMac.

The AI runs directly on your machine with full access to your files. You can say "clean up my Downloads folder" or "summarize that PDF I just saved." Simple to set up, nothing extra to manage.

**Trade-off:** The AI has access to everything you do. Use a dedicated shared folder to keep things organized.

### Local VM (OrbStack)

Best for laptops, experimentation, or when you want a safety net.

OpenClaw runs inside a lightweight Linux VM on your Mac. Isolated from your personal files, easy to delete and start fresh if something goes wrong. Great for learning or demos.

**Trade-off:** No direct file access—you'd need to copy files into the VM.

### Cloud VPS (DigitalOcean + Tailscale)

Best when you need access from anywhere—phone, tablet, multiple computers.

OpenClaw runs on a server in the cloud, always on. Tailscale creates a secure private connection so only you can reach it. DigitalOcean offers a 1-click deployment option.

**Trade-off:** Monthly cost ($12-24) plus you're managing a remote server.

---

## Resources

- [OpenClaw Website](https://openclaw.ai)
- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Tailscale](https://tailscale.com)
- [OrbStack](https://orbstack.dev)
- [DigitalOcean](https://digitalocean.com)
