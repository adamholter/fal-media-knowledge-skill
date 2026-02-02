# Audio API Reference

## Text-to-Speech: Qwen3-TTS

Best cost-to-performance TTS. Extremely cheap with extremely good voice cloning. Word pronunciation can be slightly off sometimes, but still the best option.

### TTS (1.7B)

```bash
curl -X POST "https://fal.run/fal-ai/qwen-3-tts/text-to-speech/1.7b" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello world.",
    "voice": "Vivian",
    "language": "English"
  }'
```

### Voice Cloning (2-Step Process)

**Step 1: Create speaker embedding**

```bash
curl -X POST "https://fal.run/fal-ai/qwen-3-tts/clone-voice/1.7b" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "audio_url": "https://example.com/your_reference_audio.mp3",
    "reference_text": "Optional, but helps if you have it."
  }'
```

Take `speaker_embedding.url` from the response.

**Step 2: Use speaker embedding for TTS**

```bash
curl -X POST "https://fal.run/fal-ai/qwen-3-tts/text-to-speech/1.7b" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello world in my cloned voice.",
    "speaker_voice_embedding_file_url": "PASTE speaker_embedding.url HERE",
    "reference_text": "Optional, but helps if it matches the clone reference text."
  }'
```

### Smaller Variants (Swap Endpoint Only)

- TTS 0.6B: `https://fal.run/fal-ai/qwen-3-tts/text-to-speech/0.6b`
- Clone 0.6B: `https://fal.run/fal-ai/qwen-3-tts/clone-voice/0.6b`


## Speech-to-Text

### ElevenLabs Scribe V2

$0.008/input audio minute. Keyterms usage adds 30%.

```bash
curl -X POST "https://fal.run/fal-ai/elevenlabs/speech-to-text/scribe-v2" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "audio_url": "https://example.com/input.mp3",
    "language_code": "eng",
    "tag_audio_events": true,
    "diarize": true,
    "keyterms": ["fal.ai"]
  }'
```

### Streaming STT Turbo

$0.0008/second.

```bash
curl -X POST "https://fal.run/fal-ai/speech-to-text/turbo/stream" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "audio_url": "https://example.com/long_audio.mp3",
    "use_pnc": true
  }'
```

### Whisper V3 (Word-Level Timestamps)

```bash
curl -X POST "https://fal.run/fal-ai/whisper" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "audio_url": "https://example.com/input.mp3",
    "task": "transcribe",
    "version": "3",
    "chunk_level": "word",
    "diarize": false
  }'
```


## Music Generation: Minimax Music v2

$0.03/generation.

```bash
curl -X POST "https://fal.run/fal-ai/minimax-music/v2" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Indie folk, melancholic, introspective, longing, solitary walk, coffee shop",
    "lyrics_prompt": "[verse]...\n[chorus]..."
  }'
```
