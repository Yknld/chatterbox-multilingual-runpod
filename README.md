# Chatterbox Multilingual TTS - RunPod Serverless

RunPod serverless endpoint for Chatterbox Multilingual TTS supporting 23 languages.

**Version:** 1.0.1

## Supported Languages

Arabic (ar), Danish (da), German (de), Greek (el), English (en), Spanish (es), Finnish (fi), French (fr), Hebrew (he), Hindi (hi), Italian (it), Japanese (ja), Korean (ko), Malay (ms), Dutch (nl), Norwegian (no), Polish (pl), Portuguese (pt), Russian (ru), Swedish (sv), Swahili (sw), Turkish (tr), Chinese (zh)

## RunPod Deployment

### Docker Image Build

This repo is designed to be built directly by RunPod from GitHub:

1. Go to RunPod Console → Serverless → New Endpoint
2. Choose "Build from GitHub"
3. Configure:
   - **Repository:** `https://github.com/YourUsername/chatterbox-multilingual-runpod`
   - **Branch:** `main`
   - **Dockerfile Path:** `Dockerfile`
   - **Build Context:** `.`

### Environment Variables

Required:
- `HF_TOKEN` - HuggingFace token with access to gated models (required for Chatterbox Multilingual)

### API Usage

```bash
curl -X POST "https://api.runpod.ai/v2/YOUR_ENDPOINT_ID/runsync" \
  -H "Authorization: Bearer YOUR_RUNPOD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": {
      "text": "Hello world",
      "language": "en",
      "voice": "/app/runpod/host_voice.flac"
    }
  }'
```

### Parameters

- `text` (required): Text to synthesize (max 300 chars recommended)
- `language` (required): 2-letter language code (see supported languages above)
- `voice` (optional): Path to voice reference file or omit for default voice
- `format` (optional): Output format - `mp3` (default) or `wav`
- `exaggeration` (optional): 0.0-1.0, controls expressiveness (default: 0.5)
- `temperature` (optional): Sampling temperature (default: 0.8)
- `cfg_weight` (optional): Classifier-free guidance weight (default: 0.5)
- `seed` (optional): Random seed for reproducibility

### Included Voice Files

- `/app/runpod/host_voice.flac` - English host voice (8s)
- `/app/runpod/russian_voice.flac` - Russian voice (10s)
- `/app/runpod/female_en.flac` - English female voice
- `/app/runpod/male_en.flac` - English male voice

## Performance

- First request: ~10-15 seconds (model loading + generation)
- Subsequent requests: ~10-15 seconds per generation (multilingual model is slower than Turbo)
- Model size: ~8GB
- VRAM usage: ~10-12GB

## License

Uses [Resemble AI Chatterbox](https://github.com/resemble-ai/chatterbox) under their license terms.
