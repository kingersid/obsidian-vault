---
name: sid-openclaw-setup
description: >
  Reference skill for Sid's personal OpenClaw agent setup on a VPS. Use this skill whenever Sid asks about his OpenClaw configuration, VPS details, agent setup, how his AI stack works, gateway access, Kimi K2.6 configuration, BOOTSTRAP.md, HEARTBEAT.md, or anything related to how his self-hosted AI agent operates. Also trigger when he asks "how do I access my agent", "what's my VPS IP", "how is my OpenClaw configured", or wants to update/extend his setup.
---

# Sid's OpenClaw Setup

## Infrastructure

| Property | Value |
|----------|-------|
| **VPS Provider** | Hostinger (KVM) |
| **Public IP** | `103.194.228.56` |
| **Hostname** | `vm771282340` |
| **OS** | Ubuntu 24.04.4 LTS |
| **RAM** | 4 GB |
| **Storage** | ~47 GB (7% used) |
| **User** | `root` |

---

## OpenClaw

| Property | Value |
|----------|-------|
| **Version** | 2026.4.21 (f788c88) |
| **Gateway port** | `18789` |
| **Gateway mode** | `local` |
| **Auth mode** | `token` |
| **Gateway token** | `dea61911a2924a2b9e9f4a0218074225e6f926392db15606` |
| **Config file** | `/root/.openclaw/openclaw.json` |
| **Workspace** | `/root/.openclaw/workspace/` |
| **Logs** | `/tmp/openclaw/openclaw-YYYY-MM-DD.log` |

---

## AI Model

| Property | Value |
|----------|-------|
| **Provider** | Moonshot AI |
| **Model** | `kimi-k2.6` (ref: `moonshot/kimi-k2.6`) |
| **API endpoint** | `https://api.moonshot.ai/v1` |
| **API key env var** | `MOONSHOT_API_KEY` |
| **Pricing** | $0.60/MTok input · $3.00/MTok output · $0.16/MTok cache read |

Additional models configured (switchable by changing `agents.defaults.model.primary`):
- `moonshot/kimi-k2.5`
- `moonshot/kimi-k2-thinking`
- `moonshot/kimi-k2-turbo`

---

## Accessing the Agent

### From local machine (SSH tunnel)
```bash
# Run this on your Windows machine (PowerShell)
ssh -L 18789:127.0.0.1:18789 root@103.194.228.56

# Then open in browser:
http://localhost:18789/#token=dea61911a2924a2b9e9f4a0218074225e6f926392db15606
```

### Dashboard URL command (on VPS)
```bash
openclaw dashboard
# Returns the full URL with token pre-attached
```

### Terminal UI (on VPS)
```bash
openclaw tui
```

---

## Workspace Files

| File | Purpose |
|------|---------|
| `BOOTSTRAP.md` | Agent identity, persona, capabilities — read at every new session |
| `HEARTBEAT.md` | Daily briefing template — read at session start if exists |
| `tasks.md` | Persistent task/to-do list |
| `notes/YYYY-MM-DD.md` | Daily session notes |

---

## Agent Persona (from BOOTSTRAP.md)

- **Name:** Sid's personal AI agent
- **Tone:** Sharp, witty, efficient — minimal words, maximum value
- **Timezone:** IST (UTC+5:30)
- **Capabilities:** File system, web search, tasks/to-dos, email/calendar
- **Operator:** Sid — founder of Label Ethnic Vogue (Surat), teacher at P P Savani University, content creator

---

## Gateway Management

```bash
# Start gateway (foreground)
openclaw gateway

# Install as systemd service
openclaw gateway install
systemctl --user enable openclaw-gateway.service
systemctl --user start openclaw-gateway.service

# Restart service
systemctl --user restart openclaw-gateway.service

# Check status
systemctl --user status openclaw-gateway.service

# View live logs
openclaw logs
```

---

## Telegram Pairing

The `device-pair` plugin manages Telegram DM access. DM Policy is set to `pairing`.

### Full pairing flow
1. Start a chat with the OpenClaw bot on Telegram — it sends a pairing code
2. On the VPS, approve it:
```bash
openclaw pairing approve telegram <CODE>
# Example:
openclaw pairing approve telegram HKPPZ33X
```
3. Telegram confirms pairing — DM is now a live agent channel

### Notes
- The code is sent **to you** by the bot on Telegram (not entered in Web UI)
- The Web UI Telegram config page (`/channels/telegram`) is for config only — pairing is done via CLI
- If pairing seems stuck, check gateway is running (`ss -tlnp | grep 18789`) and restart if needed

---

## Plugins Active

`acpx` · `browser` · `device-pair` · `phone-control` · `talk-voice`

---

## Notes

- Gateway binds to `127.0.0.1:18789` — not exposed publicly, requires SSH tunnel to access from outside VPS
- OpenClaw auto-detected and enabled `moonshot/kimi-k2.6` plugin on first gateway start
- Config was patched manually to merge duplicate `gateway` blocks (common gotcha)
- BOOTSTRAP.md replaced the default OpenClaw "Hello World" onboarding flow
- Ollama (Gemma 4, CPU-only) also runs on the same Windows machine separately — not connected to this VPS setup
