# GPT Image 2 (edit)

OpenAI's image model in edit mode: takes reference images plus a prompt. **Steps 2, 3 and 4** — the character reference sheet, the product reference sheet, and the product scale reference. All three steps feed images in, so they must run on an endpoint that accepts reference images. Never route them to the Step 1 text-only model.

| Field | Value |
|---|---|
| Model ID | `openai/gpt-image-2/edit` |
| Provider | fal.ai |
| Method | Sync (one call, finished result in the reply) |
| Type | Image, image-to-image |
| API key | `.env` → `FAL_KEY` |
| Docs | https://fal.ai/models/openai/gpt-image-2/edit/api |
| Cost | ~$0.16–$0.30 per image at high quality around 2K (billed by size + tokens; check the pricing page) |

## Endpoint

```
POST https://fal.run/openai/gpt-image-2/edit
Authorization: Key $FAL_KEY
Content-Type: application/json
```

## Request format

```json
{
  "prompt": "<verbatim template text>",
  "image_urls": ["https://…", "https://…"],
  "image_size": { "width": 2048, "height": 1152 },
  "quality": "high",
  "num_images": 1,
  "output_format": "png"
}
```

Per step:

| Step | `prompt` | `image_urls` (in this order) | `image_size` |
|---|---|---|---|
| 2. Character sheet | `references/templates/character-reference-sheet.md`, verbatim | the approved Step 1 model image | `{ "width": 2048, "height": 1152 }` (16:9) |
| 3. Product sheet | `references/templates/product-reference-sheet.md`, verbatim | the user's product photos (2–6) | `{ "width": 2048, "height": 1152 }` (16:9) |
| 4. Scale reference | `references/templates/character-product-interaction.md`, verbatim | user's scale photo, character sheet, product sheet | `{ "width": 1152, "height": 2048 }` (9:16) |

- **Verbatim means verbatim.** Paste the template body below its `# …` heading line exactly. Do not add, trim, or "improve".
- `quality` is always `"high"`. Never drop to `medium` or `low` to save money on these steps — the sheets are the identity anchors for the video.
- Use explicit `{width, height}` — the named presets (`portrait_16_9`, `landscape_16_9`) are only ~1024px on the long edge. Custom sizes must be multiples of 16, long edge ≤ 3840, aspect ≤ 3:1, total pixels 655,360–8,294,400; both sizes above satisfy this.
- Every URL in `image_urls` must be public. Local files go through `models/kie-upload.md` first. Max 16 images.

## Response handling

```json
{ "images": [ { "url": "https://…", "content_type": "image/png", "width": 2048, "height": 1152 } ] }
```

Download `images[0].url` immediately and save it flat into the generations folder.

## Notes

- If a request is rejected for content policy (common with real-looking people), say so plainly, show the provider's message, and ask the user how to proceed. Don't silently reword the verbatim template to get past a filter.
- GPT Image 2 is strong at preserving printed label text, which is why it owns the product sheet — don't swap it for a cheaper model on Step 3.
