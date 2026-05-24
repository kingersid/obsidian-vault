---
tags: [project, ai, voice, rag, local-stack, vps]
source: sci fi agent implementation.md
ingested: 2026-05-24
type: project-spec
status: planning
date: 2026-05-22
---

# Sci-Fi Voice Agent

## What
Convert existing `custom-rag` daily vitals agent → real-time conversational voice agent with sci-fi persona.
**Flow:** Speak → transcribe → LLM reasons with health data → synthesised voice responds.
**Target latency:** ~650ms to first audio word. Fully local (except NVIDIA LLM tier).

## Stack

| Layer | Tool | Notes |
|-------|------|-------|
| STT | `faster-whisper small.en` + Silero VAD | Free, local, CPU-only, INT8, 200–400ms |
| VAD | Silero VAD | End-of-utterance at 600ms silence |
| LLM | NVIDIA / custom-rag backend | Existing |
| TTS | (TBD) | Target: local, low-latency |

## Base
- Project: `custom-rag` → `rag-agent-server.js`
- Server: VPS `103.194.228.56`

## Status
- [ ] STT integration
- [ ] VAD pipeline
- [ ] LLM connector
- [ ] TTS voice synthesis
- [ ] Sci-fi persona prompt

## Related
- [[sid-openclaw-setup]]
- [[woocommerce-index]]
