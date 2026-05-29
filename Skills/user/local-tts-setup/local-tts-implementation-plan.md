# Local TTS Implementation Plan — Human-Like Speech on 4GB Laptop

**Date:** 2026-05-29
**Goal:** Replace browser TTS (robotic) with a local model that sounds natural and human-like, runs on CPU with 4GB RAM.

---

## Why Local TTS?

Browser TTS (Web Speech API) is free but sounds mechanical — flat prosody, unnatural pauses, robotic rhythm. Local open-source models in 2026 have closed this gap significantly and run entirely on-device with no cloud dependency.

---

## Recommended Models (4GB RAM, CPU-only)

### 🏆 #1 — Kokoro-82M *(Best balance)*
- **Size:** 82M parameters, ~500MB RAM
- **Speed:** 3–5× real-time on CPU (i5/Ryzen 5+)
- **Quality:** ⭐⭐⭐⭐ — natural prosody, no artifacts
- **Voices:** 26+ (American/British English, male/female)
- **Languages:** 8
- **License:** Apache 2.0
- **HuggingFace:** `hexgrad/Kokoro-82M`

### #2 — Piper TTS *(Fastest on CPU)*
- **Size:** 60–200MB per voice model
- **Speed:** Near-instant on any CPU
- **Quality:** ⭐⭐⭐ — good, slightly less natural than Kokoro
- **Voices:** 900+ English voices
- **Best for:** Real-time streaming, low-latency use cases

### #3 — Orpheus TTS *(Best quality, heavier)*
- **Size:** 3B parameters (~3–4GB RAM)
- **Quality:** ⭐⭐⭐⭐⭐ — ElevenLabs-level, supports emotions (laugh, whisper, cry)
- **Speed:** Slow on CPU — use for offline batch only
- **Best for:** Polished voiceovers, not real-time

---

## Quick Comparison Table

| Model | RAM | CPU Speed | Quality | Use Case |
|---|---|---|---|---|
| **Kokoro-82M** | ~500MB | Fast | ⭐⭐⭐⭐ | Default choice |
| **Piper** | ~200MB | Very Fast | ⭐⭐⭐ | Real-time / streaming |
| **Orpheus** | ~3–4GB | Slow | ⭐⭐⭐⭐⭐ | Offline batch / premium audio |

---

## Installation — Kokoro (Recommended)

### Prerequisites
```bash
sudo apt install espeak-ng   # Ubuntu / WSL
```

### Install
```bash
pip install kokoro
```

### Basic Usage
```python
from kokoro import KPipeline

pipeline = KPipeline(lang_code='a')  # 'a' = American English

generator = pipeline(
    "Hello, this is local TTS running on my laptop.",
    voice='af_heart'
)

for chunk in generator:
    # chunk.audio = numpy array
    # Save or play the audio chunk
    pass
```

### Available Voices
- `af_heart` — American Female (warm)
- `af_bella` — American Female (clear)
- `am_adam` — American Male
- `bf_emma` — British Female
- `bm_george` — British Male

---

## Installation — Piper (Fastest)

```bash
pip install piper-tts

# Download a voice model
piper-download --voice en_US-lessac-medium

# Generate speech from CLI
echo "Hello world" | piper --model en_US-lessac-medium --output_file out.wav
```

---

## Docker Option (Kokoro-FastAPI)

OpenAI-compatible `/v1/audio/speech` endpoint — easy to plug into any pipeline (Hermes Agent, web app, etc.):

```bash
docker run -p 8880:8880 ghcr.io/remsky/kokoro-fastapi-cpu:v0.2.0post4
```

Access at `http://localhost:8880` — Gradio UI for testing, or use the API endpoint directly.

---

## Integration Ideas

- **Hermes Agent pipeline:** Route TTS through Kokoro-FastAPI as the speech output stage
- **Label Ethnic Vogue Reels:** Batch generate voiceovers for video content
- **Content creation:** Narrate blog posts or scripts locally without ElevenLabs API costs
- **iOS app side project:** Explore on-device TTS via CoreML port of Kokoro

---

## Resources

- Kokoro HuggingFace: https://huggingface.co/hexgrad/Kokoro-82M
- Kokoro-FastAPI (Docker): https://github.com/remsky/kokoro-fastapi
- Piper TTS: https://github.com/rhasspy/piper
- Orpheus TTS: search HuggingFace for `canopylabs/orpheus-tts`

---

*Saved from Claude conversation — 2026-05-29*
