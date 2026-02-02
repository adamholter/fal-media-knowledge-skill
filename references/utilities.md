# Utilities API Reference

## Image Background Removal

### BiRefNet v2 (Higher Quality)

$0.00111/compute-second.

```bash
curl -X POST "https://fal.run/fal-ai/birefnet/v2" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/input.jpg",
    "model": "General Use (Light)",
    "operating_resolution": "1024x1024",
    "refine_foreground": true,
    "output_mask": false,
    "output_format": "png"
  }'
```

### Bria RMBG 2.0 (Flat Pricing, Lower Quality)

$0.018/request. May cost more or less than BiRefNet depending on compute time. Lower quality.

```bash
curl -X POST "https://fal.run/fal-ai/bria/background/remove" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/input.jpg"
  }'
```


## Video Background Removal: VEED Fast

$0.012/30 frames (refine ON), $0.008/30 frames (refine OFF).

```bash
curl -X POST "https://fal.run/veed/video-background-removal/fast" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "video_url": "https://example.com/input.mp4",
    "output_codec": "vp9",
    "refine_foreground_edges": true,
    "subject_is_person": true
  }'
```


## Image-to-3D: SAM 3D

$0.02/generation. Generate an image first for consistent angles, then convert to 3D.

```bash
curl -X POST "https://fal.run/fal-ai/sam-3/3d-objects" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/object.png",
    "prompt": "car",
    "export_textured_glb": false
  }'
```
