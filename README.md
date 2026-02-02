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

**Not sure?** Read [Remote Concepts](REMOTE-CONCEPTS.md) for the trade-offs between isolation and file access.

---

## Resources

- [OpenClaw Website](https://openclaw.ai)
- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Tailscale](https://tailscale.com)
- [OrbStack](https://orbstack.dev)
- [DigitalOcean](https://digitalocean.com)
