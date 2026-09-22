# Kling 3.0 — opt-in only

General-purpose video model on Kie AI. **Never used automatically, and never a silent fallback for Step 5.** Use it only when the user asks for Kling by name (e.g. for a cheaper, non-hero clip), after they have accepted the consistency warning below.

> **Consistency warning (say this before quoting):** "Kling animates from a single starting image, so it can't lock your model's face, outfit, and product together the way Seedance does. Expect more drift in the face and product across the clip. It's fine for B-roll, but I wouldn't use it for your hero video."

| Field | Value |
|---|---|
| Model ID | `kling-3.0/video` |
| Provider | Kie AI |
| Method | Async (submit, then poll) |
| Type | Video |
| API key | `.env` → `KIE_API_KEY` |
| Docs | https://docs.kie.ai/market/kling/kling-3-0 |
| Cost | Check Kie's pricing page before quoting. `std` = 720p, `pro` = 1080p |

## Endpoint

```
POST https://api.kie.ai/api/v1/jobs/createTask
Authorization: Bearer $KIE_API_KEY
Content-Type: application/json
```

## Request format

```json
{
  "model": "kling-3.0/video",
  "input": {
    "prompt": "<composed video prompt>",
    "image_urls": ["https://…start-frame…"],
    "duration": 15,
    "aspect_ratio": "9:16",
    "mode": "std",
    "sound": true
  }
}
```

- Start frame: the Step 4 scale reference (it shows the model *and* the product together). Rewrite the prompt's binding line for a single image — no `@Image2`/`@Image3`.
- `mode: "pro"` gives 1080p. It's the one route in this skill to 1080p, at the cost of consistency.
- Verify field names against the docs link before the first real run. The shape above follows the guide's Kie pattern and is not yet confirmed field by field.

## Response handling

1. The submit reply contains `data.taskId`.
2. Poll Kie's task-status endpoint (`GET https://api.kie.ai/api/v1/jobs/recordInfo?taskId=…`) every 10–15 seconds until the state is success/fail.
3. On success, read the video URL from the result, download immediately, save flat, write the sidecar log.
