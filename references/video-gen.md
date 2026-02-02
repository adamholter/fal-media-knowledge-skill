# Video Generation API Reference

## Grok Imagine Video

Best overall video generation. $0.05/sec of video. Image input adds ~$0.002. A 6s video costs ~$0.302. Video editing costs extra $0.01/sec for input video. Generates 1-15 seconds, up to 720p. Always use 720p (price does not change with resolution). Generates audio with the clip (useful for talking characters; strip audio in post if unwanted).

### Text-to-Video

```bash
curl -X POST "https://fal.run/xai/grok-imagine-video/text-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "duration": 6,
    "aspect_ratio": "16:9",
    "resolution": "720p"
  }'
```

### Image-to-Video

```bash
curl -X POST "https://fal.run/xai/grok-imagine-video/image-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR MOTION / CHANGE INSTRUCTIONS HERE",
    "image_url": "https://example.com/start_frame.png",
    "duration": 6,
    "aspect_ratio": "auto",
    "resolution": "720p"
  }'
```

### Video Edit (Video-to-Video)

Extra $0.01/sec for input video. 6s edit = ~$0.36. Can colorize, swap characters, restylize. Do not expect perfect identity consistency across totally different scenes.

```bash
curl -X POST "https://fal.run/xai/grok-imagine-video/edit-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR EDIT INSTRUCTIONS HERE",
    "video_url": "https://example.com/input.mp4",
    "resolution": "auto"
  }'
```


## Seedance 1.5 Pro

Middle ground. Tunable cost via resolution, duration, fps, and audio toggle. ~$0.26 per 720p 5s clip with audio. Without audio, cost is halved. Pricing: 1M video tokens with audio = $2.4, without = $1.2. tokens(video) = (height x width x FPS x duration) / 1024.

### Text-to-Video

```bash
curl -X POST "https://fal.run/fal-ai/bytedance/seedance/v1.5/pro/text-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "aspect_ratio": "16:9",
    "resolution": "720p",
    "duration": "5",
    "generate_audio": true,
    "enable_safety_checker": true
  }'
```

### Image-to-Video (Supports Start and End Frame)

```bash
curl -X POST "https://fal.run/fal-ai/bytedance/seedance/v1.5/pro/image-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "image_url": "https://example.com/start_frame.png",
    "end_image_url": "https://example.com/end_frame.png",
    "aspect_ratio": "16:9",
    "resolution": "720p",
    "duration": "5",
    "generate_audio": true,
    "enable_safety_checker": true
  }'
```


## LTX-2 19B Distilled

Cheapest option. $0.0008/megapixel of generated video data (width x height x frames). Example: 121 frames at 1280x720 = ~112 MP = ~$0.0896. Control cost via resolution and frame rate. Less powerful and less consistent than Grok. Does not follow instructions perfectly.

Known failure mode: told to add a hat to a character, output the same video except the last frame changed to a different character with seven fingers wearing a hat.

### Text-to-Video

```bash
curl -X POST "https://fal.run/fal-ai/ltx-2-19b/distilled/text-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "num_frames": 121,
    "video_size": "landscape_4_3",
    "fps": 25,
    "generate_audio": true
  }'
```

### Image-to-Video (Optional End Frame)

```bash
curl -X POST "https://fal.run/fal-ai/ltx-2-19b/distilled/image-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR PROMPT HERE",
    "image_url": "https://example.com/start_frame.png",
    "end_image_url": "https://example.com/end_frame.png",
    "num_frames": 121,
    "video_size": "auto",
    "fps": 25,
    "generate_audio": true
  }'
```

### Extend Video

```bash
curl -X POST "https://fal.run/fal-ai/ltx-2-19b/distilled/extend-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Continue the scene naturally, maintaining the same style and motion.",
    "video_url": "https://example.com/input.mp4",
    "num_frames": 121,
    "video_size": "auto",
    "generate_audio": true
  }'
```

### Video-to-Video

```bash
curl -X POST "https://fal.run/fal-ai/ltx-2-19b/distilled/video-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "YOUR RESTYLE / EDIT PROMPT HERE",
    "video_url": "https://example.com/input.mp4"
  }'
```

### Audio-to-Video (Optional Start/End Frame)

```bash
curl -X POST "https://fal.run/fal-ai/ltx-2-19b/distilled/audio-to-video" \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "A woman speaks to the camera",
    "audio_url": "https://example.com/input.mp3",
    "image_url": "https://example.com/start_frame.png",
    "end_image_url": "https://example.com/end_frame.png",
    "match_audio_length": true,
    "fps": 25
  }'
```
