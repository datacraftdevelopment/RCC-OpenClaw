# RCC OpenClaw Deployment Guide

Best practices and documentation for deploying [OpenClaw](https://openclaw.ai), the open-source personal AI assistant.

## What's Here

| Document | Description |
|----------|-------------|
| [REMOTE-CONCEPTS.md](REMOTE-CONCEPTS.md) | Why run OpenClaw remotely, VPN basics, and architecture overview |
| [LOCAL-NATIVE-SETUP.md](LOCAL-NATIVE-SETUP.md) | Install directly on your Mac (no isolation) |
| [LOCAL-VM-SETUP.md](LOCAL-VM-SETUP.md) | Run OpenClaw in a local VM using OrbStack |
| [HOSTED-SETUP.md](HOSTED-SETUP.md) | Deploy on DigitalOcean with Tailscale |

## Recommended Approach

**Desktop Mac (always on):** [LOCAL-NATIVE-SETUP.md](LOCAL-NATIVE-SETUP.md) is the simplest option. Direct file access, no extra infrastructure. Good for Mac Mini, Mac Studio, or iMac setups.

**Learning and demos:** [LOCAL-VM-SETUP.md](LOCAL-VM-SETUP.md) using OrbStack. Isolation without cloud costs, easy to reset.

**Always-on from anywhere:** [HOSTED-SETUP.md](HOSTED-SETUP.md) for DigitalOcean with Tailscale. Access from any device, runs 24/7.

**New to this?** Start with [REMOTE-CONCEPTS.md](REMOTE-CONCEPTS.md) to understand the trade-offs between local, VM, and cloud deployment.

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

## Which Setup Should You Pick?

| Your Situation | Recommended Setup |
|----------------|-------------------|
| Desktop Mac that's always on, want file access | [Local Native](LOCAL-NATIVE-SETUP.md) |
| Laptop, security-conscious, or experimenting | [Local VM (OrbStack)](LOCAL-VM-SETUP.md) |
| Need access from multiple devices, anywhere | [Cloud VPS (DigitalOcean)](HOSTED-SETUP.md) |
| Just want to understand the options first | [Remote Concepts](REMOTE-CONCEPTS.md) |

---

## Key Tools

- **Tailscale**: Zero-config VPN for secure remote access without exposing ports to the internet
- **OrbStack**: Lightweight VM solution for macOS—spin up a Linux environment in seconds
- **DigitalOcean**: Simple cloud hosting with 1-click OpenClaw deployment available

## Resources

- [OpenClaw Website](https://openclaw.ai)
- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Tailscale](https://tailscale.com)
- [OrbStack](https://orbstack.dev)
- [DigitalOcean](https://digitalocean.com)
