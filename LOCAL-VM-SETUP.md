# OpenClaw Local VM Setup (OrbStack)

This guide covers installing OpenClaw in a local Linux VM using OrbStack on macOS. This provides isolation without cloud costs—ideal for demos and testing.

## Prerequisites

- **OrbStack** installed: https://orbstack.dev (or `brew install orbstack`)
- **API key** from Anthropic, OpenAI, or OpenRouter

---

## Part 1: Create the VM

### Using OrbStack Desktop App

1. Open **OrbStack** app
2. Click **Linux > Machines** in the sidebar
3. Click the **+** button
4. Configure:
   - **Name:** `openclaw-demo`
   - **Distribution:** Ubuntu
   - **Version:** `24.04 LTS (Noble Numbat)` ← Use LTS, not latest
   - **Architecture:** arm64
5. Click **Create**

> **Note:** Use 24.04 LTS, not 25.10 or other versions. LTS is more stable and matches what cloud providers (DigitalOcean, etc.) use.

### Or via Command Line

```bash
orbctl create ubuntu:24.04 openclaw-demo
```

---

## Quick Start Commands

Once your VM is created, open the Terminal tab in OrbStack and run these commands:

```bash
# Install Node.js 22
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install nodejs git -y

# Install OpenClaw
sudo npm install -g openclaw@latest

# Run onboarding
openclaw onboard --install-daemon
```

---

## Part 2: Install Dependencies

Open the **Terminal** tab in OrbStack (click on your VM, then the Terminal tab).

### Step 1: Install Node.js 22

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```

```bash
sudo apt install nodejs git -y
```

### Step 2: Verify Node.js

```bash
node --version
```

Expected: `v22.x.x`

---

## Part 3: Install OpenClaw

```bash
sudo npm install -g openclaw@latest
```

Verify:

```bash
openclaw --version
```

Expected: `2026.x.x`

---

## Part 4: Run Onboarding

```bash
openclaw onboard --install-daemon
```

The wizard walks you through:
1. **Flow selection** - Choose quickstart or advanced
2. **AI provider** - Select Anthropic, OpenAI, OpenRouter, etc.
3. **API key** - Enter your key
4. **Gateway settings** - Port and authentication
5. **Channels** - Optional chat integrations (WhatsApp, Telegram, etc.)
6. **Skills** - Optional skill installation

After onboarding completes, OpenClaw launches the TUI (text interface).

---

## Part 5: Access from Your Mac

The gateway runs on `127.0.0.1:18789` inside the VM. To access it from your Mac:

### Option A: SSH Tunnel

Open a terminal on your Mac:

```bash
ssh -L 18789:127.0.0.1:18789 openclaw-demo.orb.local
```

Then open in browser:
```
http://127.0.0.1:18789
```

### Option B: Rebind Gateway to LAN

In the VM terminal:

```bash
# Stop current gateway (Ctrl+C if running)

# Reconfigure binding
openclaw config set gateway.bind lan

# Restart
openclaw gateway --port 18789 --verbose
```

Then access from Mac:
```
http://openclaw-demo.orb.local:18789
```

---

## Quick Reference

### OrbStack VM Management

| Action | Command |
|--------|---------|
| List VMs | `orbctl list` |
| Start VM | `orbctl start openclaw-demo` |
| Stop VM | `orbctl stop openclaw-demo` |
| Delete VM | `orbctl delete openclaw-demo` |
| SSH into VM | `ssh openclaw-demo.orb.local` |

### OpenClaw Commands (in VM)

| Action | Command |
|--------|---------|
| Check status | `openclaw doctor` |
| View config | `openclaw config list` |
| Start gateway | `openclaw gateway --port 18789 --verbose` |
| Chat with agent | `openclaw agent --message "Hello"` |
| Update OpenClaw | `sudo npm update -g openclaw@latest` |

---

## Demo Walkthrough (Clean Install)

For a live demo, these are the exact steps:

**1. Create VM** (OrbStack app or CLI)
```bash
orbctl create ubuntu:24.04 openclaw-demo
```

**2. Open Terminal** in OrbStack app (click VM → Terminal tab)

**3. Install Node.js**
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install nodejs git -y
node --version
```

**4. Install OpenClaw**
```bash
sudo npm install -g openclaw@latest
openclaw --version
```

**5. Run Onboarding**
```bash
openclaw onboard --install-daemon
```

**6. Access from Mac** (SSH tunnel)
```bash
ssh -L 18789:127.0.0.1:18789 openclaw-demo.orb.local
# Then open http://127.0.0.1:18789
```

---

## Cleanup

```bash
orbctl delete openclaw-demo --force
```

---

## Troubleshooting

### npm install runs out of memory

```bash
# Add swap space
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

### Permission denied on npm install

Use `sudo`:
```bash
sudo npm install -g openclaw@latest
```

### Can't connect to gateway from Mac

1. Verify gateway is running: `openclaw doctor`
2. Use SSH tunnel method (most reliable)
3. Check if port is in use: `sudo lsof -i :18789`
