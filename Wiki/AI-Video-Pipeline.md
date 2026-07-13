## 🎬 AI Video Pipeline (Lip Sync + Captions + Audio)

Local video generation pipeline: avatar image + voiceover audio → talking-head video with animated captions and studio-quality audio.

**Project location:** `/c/Users/kinge/ai-videos/`

---

### 📋 Pipeline Stages

#### 1. Lip-Sync Generation
| Method | Status | Details |
|---|---|---|
| **OpenRouter Wan 2.6 API** | ✅ Primary | Free tier, 15s max, `python src/lipsync_api.py` |
| **SadTalker → Wav2Lip** | ✅ Fallback | CPU mode, ~10-20 min per clip |

#### 2. Caption Pipeline (ffmpeg + ASS)
Located in `my-video/` subdirectory. Transcribes audio, generates animated single-word subtitles, burns into video.

| Style | Script | Effect |
|---|---|---|
| **Single-word English** | `fix-english-captions.py` | Each word springs up (140%→100%), fades, next follows |
| **Karaoke** | `generate-karaoke-subs.py` | Word-by-word gold highlight fill |
| **Hinglish** | `convert-to-hinglish.py` | Hindi → Romanized (via Whisper + indic-transliteration) |

#### 3. Audio Enhancement (ffmpeg)
Six-stage filter chain:

1. **Highpass/Lowpass** — Remove rumble & thin noise
2. **`anlmdn`** — Noise reduction (hiss/hum suppression)
3. **3-band EQ** — Boost presence (2kHz) and air (6kHz)
4. **`aecho` multi-tap** — Studio reverb (40ms/55ms/70ms delays)
5. **`dynaudnorm`** — Level normalization (louder without clipping)
6. **`alimiter`** — Hard ceiling at 1.0 (peak protection)

---

### ⚡ Quick Start (after lip-sync render)

```bash
cd /c/Users/kinge/ai-videos/my-video
source ../.venv/Scripts/activate
python fix-english-captions.py   # fix words + generate ASS
bash burn-fixed-english.sh       # burn captions
bash enhance-audio-v2.sh         # polish audio
```

### 🔧 Word Fixes Applied

| Original | Fixed |
|---|---|
| `ellipsing` | `lip sync` (split into 2 words) |
| `GPD` | `chatGPT` |

Edit `output/captions_en.json` to fix more words, then re-run `fix-english-captions.py`.

### 🗂️ Key Output Files

| File | Description |
|---|---|
| `output/lipsync_output.mp4` | Raw lip-sync output |
| `output/final_english_single_word.mp4` | With single-word captions |
| `output/final_enhanced_audio.mp4` | With processed audio |

### 🖥️ Hardware

- **GPU:** NVIDIA GTX 750 Ti (2GB VRAM) — all ML on CPU
- **CPU:** Intel i3 10th gen (4c/8t)
- **RAM:** 16 GB
- **OS:** Windows 11

### 📌 Related
- [[Custom AI Skills]]
- [[VPS Architecture Overview]]
