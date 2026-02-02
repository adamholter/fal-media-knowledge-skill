---
name: fal-media-gen
description: >
  Price-to-performance media generation via fal.ai. Use this skill for ANY media generation task
  that is not 100% programmatic, including: image generation (memes, graphics, banners, ads),
  video generation (clips, animations, edits), text-to-speech and voice cloning, speech-to-text
  transcription, music generation, background removal (image and video), image-to-3D conversion,
  and image/video upscaling (super-resolution, enhancement, enlargement).
  Never attempt media generation without reading this skill first. Covers model selection, pricing,
  API endpoints, and best practices. Do not use models outside this list.
---

# fal.ai Media Generation

## Critical Rule

Never attempt any media generation task that is not 100% programmatic without reading this skill first. Do not use models that are not in this skill's reference docs.

## API Key

Set your fal.ai API key as an environment variable before making requests:

```
export FAL_KEY=your-fal-api-key-here
```

Get your key at https://fal.ai/dashboard/keys

All requests use this key in the `Authorization: Key $FAL_KEY` header.

## Quick Model Selection

### Image Generation

| Model | Cost | Best For | Text? | References Needed? |
|---|---|---|---|---|
| **Seedream 4.5** | $0.04/image | Default choice, most tasks | Yes (perfect) | Yes, for specific brands/characters |
| **Nano Banana Pro** | ~$0.15/image at 2K | Premium quality, known brands | Yes (perfect) | No, strong world knowledge |
| **Z-Image Turbo** | $0.005/megapixel | Cheap backgrounds, simple graphics | Limited (1-2 sentences) | No image editing support |

Decision flow:
1. Need known brand logos or maximum quality? -> Nano Banana Pro
2. Standard task, good quality needed? -> Seedream 4.5 (provide reference images for specific entities)
3. Simple background or cheap bulk generation? -> Z-Image Turbo

### Video Generation

| Model | Cost | Best For |
|---|---|---|
| **Grok Imagine Video** | $0.05/sec + small input fees | Best quality, includes audio, up to 15s at 720p |
| **Seedance 1.5 Pro** | ~$0.26 per 720p 5s clip w/ audio | Middle ground, tunable cost via resolution/fps/audio toggle |
| **LTX-2 19B Distilled** | $0.0008/megapixel of video data | Cheapest, control cost via resolution/fps, less consistent |

Decision flow:
1. Best quality, budget allows $0.05/sec? -> Grok Imagine Video (always use 720p)
2. Need cheaper but decent? -> Seedance 1.5 Pro (turn off audio to halve cost)
3. Pinching pennies, lower quality acceptable? -> LTX-2 19B Distilled

### Audio

| Task | Model | Cost |
|---|---|---|
| **TTS** | Qwen3-TTS 1.7B | Extremely cheap, best cost-to-performance |
| **Voice cloning** | Qwen3-TTS 1.7B (2-step) | Create embedding, then use for TTS |
| **STT (general)** | ElevenLabs Scribe V2 | $0.008/min |
| **STT (streaming)** | Streaming STT Turbo | $0.0008/sec |
| **STT (word timestamps)** | Whisper V3 | Per-request |
| **Music** | Minimax Music v2 | $0.03/generation |

### Utilities

| Task | Model | Cost |
|---|---|---|
| **Image bg removal (quality)** | BiRefNet v2 | $0.00111/compute-sec |
| **Image bg removal (flat price)** | Bria RMBG 2.0 | $0.018/request |
| **Video bg removal** | VEED Fast | $0.012/30 frames (refine ON) |
| **Image-to-3D** | SAM 3D | $0.02/generation |

### Image/Video Upscaling

| Model | Cost | Best For |
|---|---|---|
| **AuraSR** | Free ($0/compute-sec) | Naturalistic detail, 4x only, fast GAN-based |
| **SeedVR2 Image** | $0.001/megapixel | Default upscaler, up to 10K, clean/sharp |
| **ESRGAN** | $0.000575/compute-sec | Classic, reliable, anime variants available |
| **Topaz Image** | $0.08 for ≤24MP output | Premium, face enhancement, multiple models |
| **Crystal Upscaler** | $0.016/megapixel | Portrait-specialized, up to 200x scale |
| **SeedVR2 Video** | $0.001/MP of video data | Default video upscaler, temporally consistent, up to 4K |
| **Bytedance Video** | $0.0072-$0.0288/s | Mid-range, resolution-tiered pricing |
| **Topaz Video** | $0.01-$0.08/s by resolution | Premium video, face enhancement |

Decision flow:
1. Free upscale, preserve natural detail? -> AuraSR (4x only)
2. Standard "make bigger and cleaner"? -> SeedVR2 Image
3. Portraits or faces? -> Crystal Upscaler or Topaz Image
4. Video upscale, budget? -> SeedVR2 Video
5. Video upscale, mid-range? -> Bytedance Video
6. Video upscale, premium? -> Topaz Video

## Consistent Characters Workflow

For consistent characters across video clips:
1. Generate character image with Seedream 4.5 or Nano Banana Pro
2. Use that image as start frame in image-to-video (Grok Imagine Video or Seedance 1.5 Pro)

## API Reference Files

Read the appropriate reference file before making API calls:

- **Image generation**: See [references/image-gen.md](references/image-gen.md) for Nano Banana Pro, Seedream 4.5, Z-Image Turbo endpoints
- **Video generation**: See [references/video-gen.md](references/video-gen.md) for Grok Imagine Video, LTX-2 19B, Seedance 1.5 Pro endpoints
- **Audio (TTS/STT/Music)**: See [references/audio.md](references/audio.md) for Qwen3-TTS, ElevenLabs Scribe, Whisper, Minimax Music endpoints
- **Utilities**: See [references/utilities.md](references/utilities.md) for background removal, 3D, video bg removal endpoints
- **Upscaling**: See [references/upscaling.md](references/upscaling.md) for AuraSR, SeedVR2, ESRGAN, Topaz, Crystal Upscaler, Bytedance endpoints (image and video)

## Making Requests

All fal.ai requests follow this pattern:

```bash
curl -X POST "https://fal.run/{endpoint}" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{...}'
```

Responses return JSON. For media outputs, look for `url` fields in the response containing the generated asset URL.

## Budget Awareness

When given a budget, calculate costs before generating. Example: if budget is $1.00 and you need 5 images + a 6s video clip:
- 5x Seedream 4.5 images = $0.20
- 1x Grok Imagine Video 6s = $0.302
- Total = $0.502, within budget
