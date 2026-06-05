# Sci-Fi Voice Agent Plan

> Build the `custom-rag` daily vitals agent into a fully conversational sci-fi AI — you speak, it speaks back — using a 100% free, locally-run stack.

**Date:** 2026-05-22 **Project:** `custom-rag` → voice agent upgrade **Status:** Planning

---

## Goal

Transform the existing `rag-agent-server.js` into a real-time conversational voice agent with a sci-fi persona. You speak → it transcribes → LLM reasons with your health data → synthesised voice responds. Target latency: ~650ms to first audio word. Fully local except the NVIDIA LLM tier already in use.

---

## Stack

|Layer|Tool|Why|
|---|---|---|
|**STT**|`faster-whisper small.en` + Silero VAD|Free, local, CPU-only, INT8, 200–400ms|
|**VAD**|Silero VAD|End-of-utterance at 600ms silence, cleaner than energy thresholds|
|**LLM**|Existing NVIDIA API via `rag-agent-server.js`|No change needed, already streaming|
|**TTS**|Kokoro-FastAPI (CPU Docker)|Apache 2.0, 82M params, OpenAI-compatible API, 150–200ms first chunk|
|**Frontend**|Browser Web Audio API + WebSocket|Mic capture, PCM streaming, audio playback|

### Why this stack wins on the free constraint

- `faster-whisper` with INT8 quantisation runs 6–8× real-time on a mid-range CPU, under 1GB RAM
- Kokoro runs 3–11× real-time on CPU with no GPU needed — first audio arrives while LLM is still generating
- Zero API costs after setup — only the NVIDIA free tier for the LLM, which is already running

---

## Latency Budget

```
STT (faster-whisper small.en)  →  200–400ms
LLM TTFT (NVIDIA API)          →  150–350ms
TTS first chunk (Kokoro CPU)   →  100–200ms
─────────────────────────────────────────────
Total to first audio word      →  ~650ms
```

After first chunk, audio streams continuously and feels natural. The sci-fi framing helps — a slight pause before ARIA speaks reads as _character_, not lag.

---

## Latency Optimisation: Stream LLM → TTS

Do NOT wait for the full LLM response before sending to TTS. Pipe tokens as they arrive. Kokoro buffers until it hits punctuation or 64 tokens, then synthesises immediately. This means the user hears audio while the LLM is still mid-sentence.

---

## Build Order

### Step 1 — `stt-server.py`

Python WebSocket server wrapping `faster-whisper` + Silero VAD.

```python
# ~60 lines
# - Accepts 16-bit PCM mono @ 16kHz over WebSocket
# - Silero VAD detects end-of-utterance at 600ms silence
# - faster-whisper small.en INT8 transcribes the chunk
# - Returns JSON: {"text": "...", "final": true}
```

Install:

```bash
pip install faster-whisper silero-vad fastapi uvicorn websockets
```

### Step 2 — Kokoro Docker

```bash
docker run -p 8880:8880 ghcr.io/remsky/kokoro-fastapi-cpu
```

- Test UI at `http://localhost:8880/web`
- OpenAI-compatible API at `http://localhost:8880/v1/audio/speech`
- Pick voice: `af_sky` or `am_echo` for sci-fi tone

### Step 3 — Modify `rag-agent-server.js`

Add two things:

1. A `/api/tts` endpoint that proxies text → Kokoro and streams audio bytes back
2. Pipe streamed LLM tokens → Kokoro as sentences complete (on punctuation)

The `callNvidiaLLM` function already streams — intercept chunks and forward to Kokoro mid-stream.

### Step 4 — Update `rag-chat.html`

Replace the text input flow with:

- Mic → WebSocket → `stt-server.py` → transcribed text → existing LLM flow
- LLM response → `/api/tts` → stream audio to `AudioContext` for playback
- Barge-in: stop TTS playback immediately when VAD fires on mic input

### Step 5 — Persona prompt

Rewrite the `systemPrompt` in `rag-agent-server.js`:

```js
const systemPrompt = `You are ARIA — Adaptive Reasoning and Intelligence Assistant.
You speak in short, precise sentences. Calm. Slightly clinical. Never verbose.
You know ${firstName}'s health data intimately. You reference it without being asked.
Current time: ${currentDateTime}
You do not say "I" — you refer to yourself as ARIA when needed.
When asked about sleep: frame it as a systems report. "Sleep debt: 1.4 hours. Recommend correction."
...`
```

### Step 6 — HUD frontend

Replace `rag-chat.html` with a full-screen dark interface:

- Scanning waveform while listening (Web Audio API `AnalyserNode`)
- Pulsing ring while ARIA is "thinking"
- Monospace font, streaming transcript overlay
- Silhouette or avatar synced to audio amplitude

---

## Voice Selection

Kokoro ships 54 voices. Best candidates for sci-fi AI feel:

- `af_sky` — clear, neutral, slightly formal female
- `am_echo` — measured, low-warmth male
- `bf_emma` — British, precise

No cloning needed initially. If you want a custom voice later: Coqui XTTS v2 is already installed for Priya's pipeline — point it at a reference clip.

---

## Barge-In Handling

When the user speaks while ARIA is talking:

1. Echo cancellation: browser AEC (enable via `echoCancellation: true` in `getUserMedia`)
2. VAD fires → immediately cancel active `AudioBufferSourceNode`
3. Clear TTS queue
4. Send new STT result to LLM

Target barge-in stop latency: <200ms.

---

## Files to Create / Modify

```
custom-rag/
├── stt-server.py          ← NEW: faster-whisper WebSocket server
├── rag-agent-server.js    ← MODIFY: add /api/tts, stream tokens to Kokoro
├── rag-chat.html          ← MODIFY: voice I/O, HUD layout
└── .env                   ← ADD: KOKORO_URL=http://localhost:8880
```

---

## What Stays the Same

- All existing tool-calling (`query_vitals`, `web_search`, `execute_command`)
- Supabase health data pipeline
- Skills system (`health-analysis`, `vitals-query`, `obsidian-notes`)
- Conversation history per session
- NVIDIA LLM backend

---

## Next Actions

- [ ] Write `stt-server.py` (~60 lines)
- [ ] `docker pull ghcr.io/remsky/kokoro-fastapi-cpu` and test
- [ ] Add Kokoro proxy endpoint to `rag-agent-server.js`
- [ ] Pipe streaming LLM tokens → Kokoro
- [ ] Rewrite `rag-chat.html` for voice I/O
- [ ] Write ARIA persona prompt
- [ ] Build sci-fi HUD

---

## References

- Kokoro-FastAPI CPU: https://github.com/remsky/kokoro-fastapi
- faster-whisper: https://github.com/SYSTRAN/faster-whisper
- Silero VAD: https://github.com/snakers4/silero-vad
- Kokoro voices: https://huggingface.co/hexgrad/Kokoro-82M/blob/main/VOICES.md