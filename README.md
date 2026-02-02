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
