# Image Generation API Reference

## Nano Banana Pro

Gold standard quality. ~$0.15/image at 2K. Strong world knowledge (handles brand logos, known characters without references). Only use if cheaper models cannot handle the task.

### Text-to-Image

```bash
curl -X POST "https://fal.run/fal-ai/nano-banana-pro" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "resolution": "2K",
    "aspect_ratio": "1:1",
    "num_images": 1,
    "output_format": "png"
  }'
```

### Edit (Image-to-Image)

```bash
curl -X POST "https://fal.run/fal-ai/nano-banana-pro/edit" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR EDIT INSTRUCTIONS HERE",
    "image_urls": ["https://example.com/input.png"],
    "resolution": "2K",
    "aspect_ratio": "auto",
    "num_images": 1,
    "output_format": "png"
  }'
```


## Seedream 4.5

Best default for cost. $0.04/image. Perfect text rendering. Less world knowledge than Nano Banana Pro, so provide reference images for specific brands/characters that are not universally known (e.g., Mario works fine without references, but Anthropic logo does not).

### Text-to-Image

```bash
curl -X POST "https://fal.run/fal-ai/bytedance/seedream/v4.5/text-to-image" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "image_size": "auto_2K",
    "num_images": 1,
    "max_images": 1,
    "enable_safety_checker": true
  }'
```

### Edit (Provide Reference Images)

```bash
curl -X POST "https://fal.run/fal-ai/bytedance/seedream/v4.5/edit" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR EDIT INSTRUCTIONS HERE",
    "image_urls": ["https://example.com/ref1.png", "https://example.com/ref2.png"],
    "image_size": "auto_2K",
    "num_images": 1,
    "max_images": 1,
    "enable_safety_checker": true
  }'
```


## Z-Image Turbo

Ultra-cheap. $0.005/megapixel (charged per megapixel, not per image). Good for backgrounds, banners, simple graphics. Can handle some text (1-2 sentences) but not reliably for longer text. No instruction-based image editing.

### Text-to-Image

```bash
curl -X POST "https://fal.run/fal-ai/z-image/turbo" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "image_size": "landscape_4_3",
    "num_inference_steps": 8,
    "num_images": 1,
    "output_format": "png",
    "acceleration": "regular",
    "enable_safety_checker": true
  }'
```
