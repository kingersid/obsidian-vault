## 🤖 Custom AI Skills & Agents

A collection of AI-powered systems running on the VPS for automation, analysis, and communication.

---

### 🧠 Mastra Agent Framework

- **Location:** /root/mastra-agent/
- **Service:** mastra-agent.service
- **Stack:** Node.js (Mastra framework)
- **Purpose:** AI agent orchestration server
- **Backup:** /root/mastra-agent.bak/
- **Bridges to:** Discord bot, Telegram bot (via PM2)

---

### 🌐 Hermes Agent Gateway

- **Location:** /root/hermes-webui/
- **Service:** hermes-gateway.service
- **Stack:** Python
- **Purpose:** Multi-platform messaging integration gateway
- **Features:** MCP server, agent routing, multi-platform support

---

### 🏥 Custom RAG System

- **Location:** /root/custom-rag/
- **Stack:** JavaScript (Node.js)
- **Purpose:** Retrieval-Augmented Generation for health data + dashboards
- **Key files:**
  - rag-agent-server.js — RAG agent server
  - rag-agent-utils.js — RAG utilities
  - health-agent-server.js — Health tracking agent
  - health-agent.js — Health analysis logic
  - dashboard-server.js — Dashboard server
  - voice-io-integration.js — Voice integration
  - migrate-to-supabase.js — Supabase migration
  - analyze-vitals.js — Vital analysis
- **Database:** SQLite (vitals.db)

---

### 🎙️ Voice Agents

#### 1. Python Voice Agent
- **Location:** /root/voice-agent/
- **Stack:** Python (LiveKit-based)
- **Purpose:** Voice-based AI assistant
- **Setup:** Virtual environment in .venv

#### 2. LiveKit Voice Agent
- **Location:** /root/livekit-voice-agent/
- **Stack:** JavaScript (Node.js)
- **Purpose:** Real-time voice agent using LiveKit
- **Config:** .env with LiveKit credentials

---

### 📈 Stock & Investment Skills

- **Location:** /root/awesome-stock-skills/skills/
- **Skills available:**
  - fetch-concalls — Fetch earnings call transcripts
  - growth-trigger-analysis — Growth trigger detection
  - nlm-skill — Natural language model skill
  - stock-research-pipeline — Full stock research pipeline
  - stock-research-publish — Publish stock research
- **Also:** /root/workspace/stock-dashboard/ — Stock data dashboard server

---

### 🗓️ Content Calendar & Voiceovers

- **Location:** /root/content-calendar/
- **Files:**
  - generate_all_voiceovers.py — Batch voiceover generation
  - generate_voiceover.py — Single voiceover generation
  - strategy.md — Content strategy
  - ideas_bank.md — Content ideas
  - index.html — Calendar view
- **Uses:** Sarvam AI API for voice generation

---

### 📚 Obsidian MCP Server

- **Location:** /root/obsidian-mcp-server/
- **Service:** obsidian-mcp.service
- **Stack:** Python
- **Purpose:** Model Context Protocol server for Obsidian — allows AI agents to read/write to the Obsidian vault
- **Also:** Synced vault at /root/obsidian-vault/ (git-synced every 5 minutes)

---

### 📄 File Management Systems

- /root/files-vault/ — Notes, templates, uploads
- /root/filesmd/ — File management server app (Go binary)
- filesmd-server — Binary executable for file management

---

### 📌 Related
- [[VPS Architecture Overview]]
- [[Services & Projects]]
- [[Docker & Infrastructure]]
