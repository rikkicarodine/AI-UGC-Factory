# Nano Banana 2

Google's fast photoreal image model. **Step 1 only** — the text-only model image. Picked for Step 1 because it is strong at photoreal people, cheap, and renders natively at 2K in 9:16.

| Field | Value |
|---|---|
| Model ID | `fal-ai/nano-banana-2` |
| Provider | fal.ai |
| Method | Sync (one call, finished result in the reply) |
| Type | Image, text-to-image |
| API key | `.env` → `FAL_KEY` |
| Docs | https://fal.ai/models/fal-ai/nano-banana-2/api |
| Cost | ~$0.12 per image at 2K (0.5K $0.06 · 1K $0.08 · 4K $0.16) |

## Endpoint

```
POST https://fal.run/fal-ai/nano-banana-2
Authorization: Key $FAL_KEY
Content-Type: application/json
```

## Request format

```json
{
  "prompt": "<full layered prose prompt from model-image-builder.md>",
  "aspect_ratio": "9:16",
  "resolution": "2K",
  "num_images": 1,
  "output_format": "png"
}
```

- `aspect_ratio` is always `"9:16"` on Step 1.
- `resolution` is always `"2K"` (options: `0.5K`, `1K`, `2K`, `4K`).
- The negative-prompt line from `model-image-builder.md` is appended to the end of `prompt` as a final sentence (`Avoid: …`). There is no separate negative field.
- **One variation per call** (`num_images: 1`), run one at a time. Variations must differ, and calling one-by-one avoids rate limits (guide rule). Vary the scene/pose slightly per variation only if the user asked for variety; otherwise re-send the same prompt.

## Response handling

```json
{ "images": [ { "url": "https://…", "content_type": "image/png" } ], "description": "…" }
```

Download `images[0].url` immediately — fal result URLs are not permanent — and save it flat into the generations folder (see SKILL.md → Output).

## Lite variant (cheap exploration only)

`fal-ai/nano-banana-2-lite` — same request shape, cheaper. Offer it only when the user wants many throwaway looks (e.g. "show me 10 quick options"). Once they pick a *direction*, regenerate on full Nano Banana 2. **Never "upscale" a chosen draft by rerunning its prompt** — a rerun produces a different person, and Step 1's chosen image is the identity anchor for everything downstream.

## Notes

- No reference images on this step. If the user wants to base the model on a real photo they own, that is an edit, not a text-to-image job — route it to `fal-ai/nano-banana-2/edit` with `image_urls`, and tell them.
- The fal model id has changed before. If a call returns "model not found", check https://fal.ai/models?keywords=nano%20banana and update this file.
- Web-search grounding is off by default. Leave it off — it costs an extra $0.015 and adds nothing to a portrait.
