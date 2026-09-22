# Kie AI file upload

Not a generator — a utility. fal.ai's `image_urls` fields need **public URLs**, but the user's product photos, scale photo, and your own saved sheets are local files. This turns a local file into a temporary public URL.

| Field | Value |
|---|---|
| Endpoint | `https://kieai.redpandaai.co/api/file-stream-upload` |
| Provider | Kie AI |
| Method | Sync |
| Type | Utility (upload) |
| API key | `.env` → `KIE_API_KEY` |
| Docs | https://docs.kie.ai/file-upload-api/quickstart |
| Cost | Free with a Kie account |

## Endpoint

```
POST https://kieai.redpandaai.co/api/file-stream-upload
Authorization: Bearer $KIE_API_KEY
Content-Type: multipart/form-data
```

## Request format

Multipart form fields:

| Field | Value |
|---|---|
| `file` | the local file (binary) |
| `uploadPath` | `aiugc` (a folder name on Kie's side — keep it fixed) |
| `fileName` | the local file's basename, e.g. `glowserum_product-sheet_1774912000.png` |

```bash
curl -s -X POST https://kieai.redpandaai.co/api/file-stream-upload \
  -H "Authorization: Bearer $KIE_API_KEY" \
  -F "file=@generations/refs/glowserum_product-front.jpg" \
  -F "uploadPath=aiugc" \
  -F "fileName=glowserum_product-front.jpg"
```

## Response handling

```json
{
  "success": true,
  "code": 200,
  "msg": "…",
  "data": {
    "fileName": "…",
    "filePath": "…",
    "downloadUrl": "https://tempfile.redpandaai.co/…/aiugc/glowserum_product-front.jpg",
    "fileSize": 123456,
    "mimeType": "image/jpeg",
    "uploadedAt": "…"
  }
}
```

Use `data.downloadUrl` as the public URL. Check `success` is `true` before using it.

## Notes

- **Uploads expire after 3 days.** Don't store these URLs as if they were permanent. Re-upload the local file whenever a step needs it again, e.g. a new video days later.
- A generated image's fal result URL can be passed straight into the next step's `image_urls` without re-uploading, **but only within the same session** (fal URLs expire too). Across sessions, always re-upload from the saved local file.
- Other variants exist if needed: `…/api/file-base64-upload` (JSON body with a base64 string or data URL) and `…/api/file-url-upload` (Kie downloads a remote URL for you).
- **Fallback when `KIE_API_KEY` is missing:** fal.ai also hosts uploads through its own storage (the official fal client's `storage.upload`). If only `FAL_KEY` is set, use that route if the fal client is available. Otherwise ask the user for a Kie key. Say which route ran.
