# OpenClaw Hosted Setup Guide

This guide covers deploying OpenClaw on a remote VPS with secure access via Tailscale.

## Prerequisites

- Tailscale account (free tier works): https://tailscale.com
- DigitalOcean account: https://digitalocean.com
- API key from your AI provider (Anthropic, OpenAI, etc.)

---

## Part 1: VPS Provisioning (DigitalOcean)

### Option A: 1-Click Deploy (Easiest)

1. Go to [DigitalOcean Marketplace - OpenClaw](https://marketplace.digitalocean.com/apps/openclaw)
2. Click "Create OpenClaw Droplet"
3. Select region closest to you
4. Choose plan: **4GB RAM minimum** ($24/month)
5. Add SSH key or choose password
6. Create Droplet

The 1-Click deploy includes:
- OpenClaw pre-installed in Docker container
- Security hardening (firewall, non-root execution)
- Auto-generated gateway authentication token

### Option B: Manual Setup (More Control, $12/month)

#### 1. Create Droplet

- **Image:** Ubuntu 24.04 LTS
- **Plan:** Premium AMD, 2GB RAM, 1 vCPU, 50GB NVMe
- **Region:** Nearest to you
- **Authentication:** SSH key (recommended)

#### 2. Initial Server Setup

```bash
# SSH into your new server
ssh root@YOUR_DROPLET_IP

# Create non-root user
adduser clawuser
usermod -aG sudo clawuser

# Switch to new user
su - clawuser
```

#### 3. Install Node.js 22+

```bash
# Install Node.js via NodeSource
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# Verify
node --version  # Should show v22.x.x
```

#### 4. Install OpenClaw

```bash
# For limited RAM systems, set memory limit
export NODE_OPTIONS="--max-old-space-size=2048"

# Install OpenClaw globally
npm install -g openclaw@latest

# Reload shell
exec bash

# Run onboarding wizard
openclaw onboard --install-daemon
```

The wizard will:
- Configure your AI provider (Anthropic/OpenAI)
- Set up gateway authentication
- Install systemd service for auto-start

#### 5. Start the Gateway

```bash
# Start with verbose output (for testing)
openclaw gateway --port 18789 --verbose

# Or let systemd manage it
sudo systemctl status openclaw-gateway
```

---

## Part 2: Secure Access with Tailscale

Tailscale creates an encrypted mesh network between your devices. No port forwarding or firewall holes needed.

### Install Tailscale on VPS

```bash
# On your DigitalOcean droplet
curl -fsSL https://tailscale.com/install.sh | sh

# Authenticate (opens browser URL)
sudo tailscale up

# Note your Tailscale IP (100.x.x.x)
tailscale ip -4
```

### Install Tailscale on Your Mac

```bash
# Install via Homebrew
brew install tailscale

# Or download from https://tailscale.com/download/mac

# Start and authenticate
sudo tailscale up
```

### Verify Connection

```bash
# From your Mac, ping the droplet's Tailscale IP
ping 100.x.x.x  # Your droplet's Tailscale IP

# SSH via Tailscale (no public IP needed)
ssh clawuser@100.x.x.x
```

### Enable Tailscale SSH (Optional but Recommended)

Tailscale can manage SSH authentication, eliminating the need for SSH keys:

```bash
# On the VPS
sudo tailscale up --ssh

# Now you can SSH using Tailscale identity
ssh clawuser@YOUR_DROPLET_TAILSCALE_HOSTNAME
```

---

## Part 3: Configure OpenClaw for Tailscale

OpenClaw has native Tailscale integration. Edit `~/.openclaw/openclaw.json`:

```json
{
  "agent": {
    "model": "anthropic/claude-sonnet-4-20250514"
  },
  "gateway": {
    "port": 18789,
    "tailscale": {
      "mode": "serve"
    }
  }
}
```

**Tailscale modes:**
- `off` - No Tailscale automation (default)
- `serve` - HTTPS access within your tailnet only
- `funnel` - Public HTTPS access (requires password auth)

For private use, `serve` is recommended.

### Access the Dashboard

With Tailscale configured, access from your Mac:

```
https://YOUR_DROPLET_TAILSCALE_HOSTNAME:18789
```

Or via SSH tunnel (if not using Tailscale serve):

```bash
# Create tunnel
ssh -L 18789:127.0.0.1:18789 clawuser@100.x.x.x

# Then open in browser
open http://127.0.0.1:18789
```

---

## Part 4: File Transfer

### Option A: Taildrop (Quick Transfers)

Built into Tailscale. Encrypted, peer-to-peer, supports resume.

**From Mac to VPS:**
```bash
# Send a file
tailscale file cp /path/to/file YOUR_DROPLET_HOSTNAME:

# Files land in ~/Taildrop/ on the VPS
```

**From VPS to Mac:**
```bash
# On VPS
tailscale file cp /path/to/file YOUR_MAC_HOSTNAME:

# Files land in ~/Downloads/ on Mac
```

### Option B: rsync over Tailscale (Directory Sync)

Use standard rsync with Tailscale's secure connection:

```bash
# Sync local directory to VPS
rsync -avz /local/path/ clawuser@100.x.x.x:/remote/path/

# Sync VPS directory to local
rsync -avz clawuser@100.x.x.x:/remote/path/ /local/path/
```

### Option C: Taildrive (Persistent WebDAV)

For ongoing file access, Taildrive provides WebDAV folders:

```bash
# On VPS, share a directory
tailscale drive share myfiles /home/clawuser/shared

# On Mac, access via Finder or mount
# Go > Connect to Server > tailscale://YOUR_DROPLET_HOSTNAME/myfiles
```

---

## Part 5: Useful Commands

### OpenClaw

```bash
# Check status
openclaw doctor

# View dashboard token
openclaw dashboard

# Send a test message
openclaw agent --message "Hello from the cloud"

# Update to latest version
openclaw update --channel stable

# View logs
journalctl -u openclaw-gateway -f
```

### Tailscale

```bash
# Check connection status
tailscale status

# View your Tailscale IP
tailscale ip -4

# List devices on your tailnet
tailscale status --peers

# Disconnect temporarily
sudo tailscale down

# Reconnect
sudo tailscale up
```

### Systemd (Service Management)

```bash
# Check if OpenClaw is running
sudo systemctl status openclaw-gateway

# Restart the service
sudo systemctl restart openclaw-gateway

# View logs
sudo journalctl -u openclaw-gateway -f

# Enable auto-start on boot
sudo systemctl enable openclaw-gateway
```

---

## Security Considerations

1. **Never expose the gateway port (18789) publicly** - Use Tailscale or SSH tunnels only

2. **Keep the gateway token secure** - It's stored in `~/.openclaw/`

3. **Use Tailscale ACLs** for multi-user setups - Control who can access what

4. **Regular updates:**
   ```bash
   openclaw update --channel stable
   sudo apt update && sudo apt upgrade
   ```

5. **Consider UFW firewall** with Tailscale:
   ```bash
   sudo ufw allow in on tailscale0
   sudo ufw enable
   ```

---

## Estimated Costs

| Item | Monthly Cost |
|------|-------------|
| DigitalOcean Droplet (2GB) | $12 |
| DigitalOcean 1-Click (4GB) | $24 |
| Tailscale | Free (personal use) |
| Anthropic API | $30-60 (usage dependent) |
| **Total** | **$42-84** |

---

## Troubleshooting

### "Out of memory" during npm install
```bash
# Add swap space
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### Gateway won't start
```bash
# Check for port conflicts
sudo lsof -i :18789

# Check logs
journalctl -u openclaw-gateway --no-pager -n 50
```

### Tailscale can't connect
```bash
# Re-authenticate
sudo tailscale up --reset

# Check firewall
sudo ufw status
```

---

## References

- [OpenClaw Documentation](https://docs.openclaw.ai/)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [DigitalOcean OpenClaw Tutorial](https://www.digitalocean.com/community/tutorials/how-to-run-openclaw)
- [Tailscale Quick Start](https://tailscale.com/kb/1017/install)
- [Tailscale SSH](https://tailscale.com/kb/1193/tailscale-ssh)
- [Taildrop](https://tailscale.com/kb/1106/taildrop)
