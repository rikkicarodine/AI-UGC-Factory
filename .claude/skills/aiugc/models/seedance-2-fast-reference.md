# Seedance 2.0 Fast — reference to video

ByteDance's video model in reference-to-video mode: takes up to 9 images plus a prompt, generates video with native audio. **Step 5 only** — the 15-second UGC video. It is the only video option in this skill that binds several reference images at once, which is what keeps face, outfit, and product locked together for the whole clip.

| Field | Value |
|---|---|
| Model ID | `bytedance/seedance-2.0/fast/reference-to-video` |
| Provider | fal.ai (fallback: WaveSpeed AI — see Notes) |
| Method | **Async via the fal queue** (a 15s clip takes minutes; don't hold a sync connection open) |
| Type | Video, reference-to-video, with native audio |
| API key | `.env` → `FAL_KEY` |
| Docs | https://fal.ai/models/bytedance/seedance-2.0/fast/reference-to-video/api |
| Cost | **$0.2419 per second at 720p → ~$3.63 for the 15s clip** |

## Endpoint

```
POST https://queue.fal.run/bytedance/seedance-2.0/fast/reference-to-video
Authorization: Key $FAL_KEY
Content-Type: application/json
```

## Request format

```json
{
  "prompt": "<composed video prompt from video-prompt-builder.md>",
  "image_urls": [
    "https://…character-sheet…",
    "https://…product-sheet…",
    "https://…scale-reference…"
  ],
  "duration": "15",
  "resolution": "720p",
  "aspect_ratio": "9:16",
  "generate_audio": true
}
```

- **`image_urls` order is the `@ImageN` order.** Index 0 = `@Image1` = character sheet, index 1 = `@Image2` = product sheet, index 2 = `@Image3` = scale reference. If Step 4 was skipped, send two URLs and make sure the prompt has no `@Image3`.
- `duration` is always the string `"15"`. `aspect_ratio` always `"9:16"`. `generate_audio` always `true` — the prompt's sound direction depends on it.
- `resolution` is `"720p"`. **This endpoint does not offer 1080p** (options are `480p` and `720p`). See Notes if the user asks for 1080p.
- Limits: images JPEG/PNG/WebP, ≤ 30 MB each, ≤ 9 images; ≤ 12 files total across images/videos/audio. All URLs must be public — local files go through `models/kie-upload.md`.
- Optional `seed` (integer): record it from the result into the sidecar log so a good take can be reproduced.

## Response handling (queue pattern)

1. The submit reply contains `request_id`, `status_url`, and `response_url`.
2. `GET status_url` (same `Authorization` header) every 10–15 seconds. States: `IN_QUEUE` → `IN_PROGRESS` → `COMPLETED`.
3. When `COMPLETED`, `GET response_url`:
   ```json
   { "video": { "url": "https://…mp4" }, "seed": 123456 }
   ```
4. **Download `video.url` immediately** — result URLs expire.
5. Save it flat into the generations folder, then write the sidecar log.

Be patient. Tell the user it's rendering and roughly how long it may take; do not resubmit because it feels slow — every submit is a new paid run.

## Notes

- **1080p:** not available on this fal endpoint. If the user asks, say so plainly. WaveSpeed AI lists Seedance 2.0 tiers that reach 1080p (roughly $4.50 for a 15s clip at 1080p). Confirm that WaveSpeed's variant accepts multiple reference images before offering it. If it only takes one start image, it has the same consistency problem as Kling (below). Quote the price and get approval first.
- **Standard (non-Fast) tier:** `bytedance/seedance-2.0/reference-to-video`, same request shape, $0.3024/s (~$4.54 for 15s), also 480p/720p. Offer it only if the user asks for higher quality than Fast.
- A content-policy rejection is not a transient error. Show the message, don't retry automatically.
