# RCC OpenClaw Deployment Guide

Best practices and documentation for deploying [OpenClaw](https://openclaw.ai), the open-source personal AI assistant.

## What's Here

| Document | Description |
|----------|-------------|
| [REMOTE-CONCEPTS.md](REMOTE-CONCEPTS.md) | Why run OpenClaw remotely, VPN basics, and architecture overview |
| [LOCAL-VM-SETUP.md](LOCAL-VM-SETUP.md) | Step-by-step guide for running OpenClaw in a local VM using OrbStack |
| [HOSTED-SETUP.md](HOSTED-SETUP.md) | Full deployment guide for DigitalOcean with Tailscale |

## Recommended Approach

**For learning and demos:** Start with [LOCAL-VM-SETUP.md](LOCAL-VM-SETUP.md) using OrbStack. Zero cost, quick setup, great for testing.

**For production use:** Follow [HOSTED-SETUP.md](HOSTED-SETUP.md) to deploy on DigitalOcean with Tailscale for secure remote access.

**New to this?** Read [REMOTE-CONCEPTS.md](REMOTE-CONCEPTS.md) first to understand why we recommend running OpenClaw in an isolated environment rather than directly on your personal machine.

## Key Concepts

- **Isolation**: Run OpenClaw in a VM or VPS to keep your personal data separate from the AI agent
- **Tailscale**: Zero-config VPN for secure access without exposing ports to the internet
- **OrbStack**: Lightweight VM solution for macOS that makes local testing easy

## Resources

- [OpenClaw Website](https://openclaw.ai)
- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [Tailscale](https://tailscale.com)
- [OrbStack](https://orbstack.dev)
- [DigitalOcean](https://digitalocean.com)
