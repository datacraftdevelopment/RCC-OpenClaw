# Local Native Setup (No Isolation)

Running OpenClaw directly on your Mac without a VM or remote server.

---

## When This Makes Sense

Local native installation works well when:

- **Desktop computer**: Your Mac is always on (Mac Mini, Mac Studio, iMac)
- **File access is a feature**: You want the AI to read and work with your local files
- **Simplicity**: You don't want to manage VMs or cloud servers
- **Single user**: It's your personal machine, not shared

---

## When to Consider Isolation Instead

A VM or remote server is better when:

- **Laptop**: Your computer sleeps, travels, or isn't always available
- **Security-sensitive work**: You handle credentials, client data, or sensitive documents
- **Experimentation**: You're testing prompts that might do unexpected things
- **Shared computer**: Others use the same machine

---

## Advantages

| Benefit | Why It Matters |
|---------|----------------|
| **Direct file access** | AI can read, edit, and organize files anywhere on your system |
| **No extra cost** | No VPS fees, no VM overhead |
| **Simpler setup** | One install, no networking to configure |
| **Better performance** | No virtualization layer |
| **Always available** | If your Mac is on, OpenClaw is on |

---

## Disadvantages

| Risk | Mitigation |
|------|------------|
| **Full system access** | Use a dedicated shared folder for AI work |
| **No sandbox** | Be thoughtful about what you ask it to do |
| **Harder to reset** | Can't just delete a VM if something goes wrong |

---

## Best Practice: Shared Folder

Create a dedicated folder for working with OpenClaw:

```
~/AI-Workspace/
├── inbox/       # Files for AI to process
├── projects/    # Active work
└── output/      # AI-generated files
```

When working with files, point the AI to this folder rather than giving it free rein across your system. This gives you the convenience of file access while limiting scope.

**Example prompt:**
> "Check the inbox folder in my AI workspace and summarize any new documents"

---

## Installation

OpenClaw installs with a single command:

```bash
npm install -g openclaw@latest
```

Then run the onboarding wizard:

```bash
openclaw onboard --install-daemon
```

The wizard walks you through:
- AI provider setup (Anthropic, OpenAI, etc.)
- API key configuration
- Gateway settings
- Optional chat integrations (Telegram, WhatsApp, etc.)

---

## Running OpenClaw

After setup, OpenClaw runs as a background service. Access it via:

- **Terminal**: `openclaw tui` for the text interface
- **Web UI**: `http://127.0.0.1:18789` in your browser
- **Chat apps**: If you configured Telegram, WhatsApp, etc.

---

## Summary

Local native installation trades isolation for convenience. For a desktop Mac that's always on, this can be the right choice—especially if file access is part of your workflow.

Just be intentional: use a shared folder, understand what you're asking the AI to do, and remember it has access to your system.
