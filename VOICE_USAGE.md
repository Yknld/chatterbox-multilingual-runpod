# Voice Usage Guide

## Available Voices

### English
- **Male:** `/app/runpod/male_en.flac` (8.9s, natural)
- **Female:** `/app/runpod/female_en.flac` (8.0s, natural)

### Russian
- **Female:** `/app/runpod/russian_voice.flac` (10s, from YouTube sample)

## API Usage

### Male Voice (English)
```bash
curl -X POST "https://api.runpod.ai/v2/YOUR_ENDPOINT_ID/runsync" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": {
      "text": "Welcome to today'\''s podcast episode.",
      "language": "en",
      "voice": "/app/runpod/male_en.flac"
    }
  }'
```

### Female Voice (English)
```bash
curl -X POST "https://api.runpod.ai/v2/YOUR_ENDPOINT_ID/runsync" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": {
      "text": "Welcome to today'\''s podcast episode.",
      "language": "en",
      "voice": "/app/runpod/female_en.flac"
    }
  }'
```

### Russian Voice (Female)
```bash
curl -X POST "https://api.runpod.ai/v2/YOUR_ENDPOINT_ID/runsync" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": {
      "text": "Привет, как дела?",
      "language": "ru",
      "voice": "/app/runpod/russian_voice.flac"
    }
  }'
```

## For Your Supabase Edge Function

When calling from your podcast generation function:

```typescript
const voice = segment.speaker === 'host' ? 'male_en.flac' : 'female_en.flac';

const response = await fetch(`${RUNPOD_API_URL}/runsync`, {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${RUNPOD_API_KEY}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    input: {
      text: segment.text,
      language: episode.language, // 'en' or 'ru'
      voice: `/app/runpod/${voice}`
    }
  })
});
```

## Adding More Voices

To add voices for other languages:

1. Get a 5-10 second audio sample of natural human speech
2. Convert to FLAC: `ffmpeg -i input.mp3 -t 8 -ac 1 -ar 24000 output.flac`
3. Place in `runpod/` directory with naming: `{gender}_{language}.flac`
4. Commit and rebuild the Docker image

Example: `male_es.flac` (Spanish male), `female_fr.flac` (French female)
