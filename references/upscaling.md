# Upscaling API Reference

## Image Upscaling

### AuraSR (Free, Naturalistic)

$0 per compute second (free). GAN-based 4x upscaler. Best for preserving natural detail and texture without the "AI-cleaned" look. Use v2 checkpoint with overlapping tiles for best quality.

```bash
curl -X POST "https://fal.run/fal-ai/aura-sr" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/input.jpg",
    "upscaling_factor": 4,
    "overlapping_tiles": true,
    "checkpoint": "v2"
  }'
```

Parameters: `upscaling_factor` (only 4), `overlapping_tiles` (true reduces seams, doubles inference time), `checkpoint` ("v1" or "v2", prefer v2).


### SeedVR2 Image (Default Cheap Upscaler)

$0.001/megapixel (output). Clean, sharp, slightly AI-styled results. Handles images up to ~10K pixels on the long side. Two modes: factor-based or target resolution.

```bash
curl -X POST "https://fal.run/fal-ai/seedvr/upscale/image" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/input.jpg",
    "upscale_mode": "factor",
    "upscale_factor": 2,
    "noise_scale": 0.1,
    "output_format": "jpg"
  }'
```

Parameters: `upscale_mode` ("factor" or "target"), `upscale_factor` (default 2, used with "factor" mode), `target_resolution` ("720p", "1080p", "1440p", "2160p", used with "target" mode), `noise_scale` (default 0.1), `output_format` ("png", "jpg", "webp").


### ESRGAN (Classic, Fast)

$0.000575/compute-second. Well-established upscaler with multiple model variants including anime-specific options. Good all-purpose choice.

```bash
curl -X POST "https://fal.run/fal-ai/esrgan" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/input.jpg",
    "scale": 2,
    "model": "RealESRGAN_x4plus",
    "face": false,
    "output_format": "png"
  }'
```

Parameters: `scale` (default 2), `model` (options: "RealESRGAN_x4plus", "RealESRGAN_x2plus", "RealESRGAN_x4plus_anime_6B", "RealESRGAN_x4_v3", "RealESRGAN_x4_wdn_v3", "RealESRGAN_x4_anime_v3"), `face` (true for face-focused), `tile` (0 = no tile, use 400 if OOM), `output_format` ("png", "jpeg").


### Topaz Image (Premium Quality)

$0.08 for up to 24MP output, $0.16 up to 48MP, $0.32 up to 96MP, up to $1.36 for 512MP. Multiple specialized models including face enhancement. Best quality available.

```bash
curl -X POST "https://fal.run/fal-ai/topaz/upscale/image" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/input.jpg",
    "model": "Standard V2",
    "upscale_factor": 2,
    "output_format": "jpeg",
    "subject_detection": "All",
    "face_enhancement": true,
    "face_enhancement_strength": 0.8,
    "face_enhancement_creativity": 0
  }'
```

Parameters: `model` (options: "Low Resolution V2", "Standard V2", "CGI", "High Fidelity V2", "Text Refine", "Recovery", "Redefine", "Recovery V2"), `upscale_factor` (default 2), `subject_detection` ("All", "Foreground", "Background"), `face_enhancement` (default true), `face_enhancement_strength` (0.0-1.0, default 0.8), `face_enhancement_creativity` (0.0-1.0, default 0), `output_format` ("jpeg", "png").


### Crystal Upscaler (Portrait Specialist)

$0.016/megapixel (output). Clarity AI's portrait-optimized upscaler. Up to 200x scale factor. Best for faces and headshots.

```bash
curl -X POST "https://fal.run/fal-ai/crystal-upscaler" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/portrait.jpg",
    "scale_factor": 2
  }'
```

Parameters: `scale_factor` (default 2, up to 200).


## Video Upscaling

### SeedVR2 Video (Default Cheap Video Upscaler)

$0.001/megapixel of video data (width x height x frames). Temporally consistent, up to 4K output. Same two modes as image variant.

```bash
curl -X POST "https://fal.run/fal-ai/seedvr/upscale/video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "video_url": "https://example.com/input.mp4",
    "upscale_mode": "target",
    "target_resolution": "1080p",
    "noise_scale": 0.1,
    "output_format": "X264 (.mp4)",
    "output_quality": "high",
    "output_write_mode": "balanced"
  }'
```

Parameters: `upscale_mode` ("factor" or "target"), `upscale_factor` (default 2), `target_resolution` ("720p", "1080p", "1440p", "2160p"), `noise_scale` (default 0.1), `output_format` ("X264 (.mp4)", "VP9 (.webm)", "PRORES4444 (.mov)", "GIF (.gif)"), `output_quality` ("low", "medium", "high", "maximum"), `output_write_mode` ("fast", "balanced", "small").


### Bytedance Video Upscaler (Mid-Range)

$0.0072/s at 1080p, $0.0144/s at 2K, $0.0288/s at 4K (at 30fps). Good balance of quality and cost.

```bash
curl -X POST "https://fal.run/fal-ai/bytedance-upscaler/upscale/video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "video_url": "https://example.com/input.mp4"
  }'
```

Parameters: `video_url` (required).


### Topaz Video (Premium Video Upscaler)

$0.01/s for up to 720p output, $0.02/s for 720p-1080p, $0.08/s for above 1080p. Best quality available with face enhancement support.

```bash
curl -X POST "https://fal.run/fal-ai/topaz/upscale/video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "video_url": "https://example.com/input.mp4",
    "upscale_factor": 2,
    "face_enhancement": true,
    "face_enhancement_strength": 0.8
  }'
```
