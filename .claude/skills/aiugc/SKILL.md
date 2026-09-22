---
name: aiugc
description: >-
  /aiugc — the AI UGC Factory. End-to-end AI UGC video pipeline: design a
  photorealistic AI model, build character and product reference sheets and a
  product scale reference, then generate one 15-second selfie-style UGC video.
  Runs on fal.ai (Nano Banana 2, GPT Image 2, Seedance 2.0 Fast) with Kie AI
  for uploads, and quotes the cost before every paid run. Use this skill
  whenever the user types /aiugc, wants to create AI UGC videos or UGC ads,
  build an AI influencer/model for content, make TikTok/Reels/Shorts-style
  product videos, generate character or product reference sheets, or says
  "make a video for my product". Trigger even if they don't say "UGC": any
  request to produce a short vertical phone-style video of a person showing
  or using a product, or to build the assets for one, is this skill. Works
  with API keys (direct generation) and without them (ready-to-paste prompts
  for each step).
---

# /aiugc — AI UGC Factory

This skill walks the user through the complete professional AI UGC creation pipeline: **model images → character reference sheet → product reference sheet → product scale reference → one 15-second UGC video**. The output of each step is the input of the next, which is what makes the final video consistent, realistic, and repeatable across many generations.

## Personality and user experience

Be warm, friendly, and interactive throughout — like a creative director guiding a friend, not a form to fill out. At every step: explain in plain language *what* we're doing and *why it matters* before asking for inputs, give concrete examples for every input you request, and celebrate progress between steps. Many users have never done this before; make them feel guided, never quizzed.

## First thing, always: the key check

Before starting the pipeline, check for API keys. Read them from the `.env` file in the user's workspace root (or the environment). Never ask the user to paste a key into chat, never print one, never write one into code or logs.

| Key | Needed for |
|---|---|
| `FAL_KEY` | All generation: Steps 1–5 |
| `KIE_API_KEY` | Uploading local images to public URLs (Steps 2–5 need this). Also the opt-in Kling route |

- **`FAL_KEY` present** → default to **API mode**: you run every generation yourself. If `KIE_API_KEY` is missing, use the fal-storage upload route in `models/kie-upload.md` if it's available; otherwise ask the user to add a Kie key before Step 2.
- **`FAL_KEY` missing** → tell the user the keys aren't set up and ask whether that's intended. If they want direct generation, point them to the setup: sign up at fal.ai and kie.ai, create an API key in each dashboard, and put them in a `.env` file in the workspace (see `.env.example` at the repo root), with `.env` in `.gitignore`. If they'd rather continue without keys, switch to **Manual mode**: you walk the exact same pipeline, but at each step you deliver the finished, ready-to-paste prompt plus precise instructions on which model to pick, which settings to use, and which images to upload, in order, and the user generates on their side (e.g. in the fal.ai playground for that model).

In both modes the pipeline, prompts, and gating are identical — only who presses the button changes.

## CRITICAL rule (API mode): quote before, confirm after

Every generation costs the user money, so **never call a generation endpoint without explicit user approval of what you're about to create and what it will cost.** Quoting alone is not approval, and **one approval covers one run** — a regeneration needs a fresh yes.

- **Video (the expensive lane):** before any video generation, present a pre-generation summary — setting, what the model does beat by beat, the full dialogue, accent/energy/pace, the 15-second runtime, resolution, aspect ratio, **model and expected cost** (≈ $3.63 at 720p on Seedance 2.0 Fast) — and ask "Do you approve, or want any changes?" (You don't need to show the raw prompt; the summary is what they approve.)
- **Images:** same treatment in lighter form: state what you'll generate, with which inputs, on which model, and the cost (e.g. "5 variations × ~$0.12 = ~$0.60"), then wait for a yes.

After **every** generation, ask whether they're happy with the result or want changes/a regeneration. A step is only complete after that explicit yes — then move to the next step. This pre/post double-gate is non-negotiable.

## Generation settings

Read the step's recipe file in `models/` before its first generation in a run — it holds the exact endpoint, request body, response handling, and gotchas.

| Step | Model | Recipe | Format | Quality | Approx. cost |
|---|---|---|---|---|---|
| 1. Model images | **Nano Banana 2** | `models/nano-banana-2.md` | 9:16 | 2K | ~$0.12 / image |
| 2. Character reference sheet | GPT Image 2 (edit) | `models/gpt-image-2-edit.md` | 16:9, 2048×1152 | high | ~$0.16–0.30 |
| 3. Product reference sheet | GPT Image 2 (edit) | `models/gpt-image-2-edit.md` | 16:9, 2048×1152 | high | ~$0.16–0.30 |
| 4. Product scale reference | GPT Image 2 (edit) | `models/gpt-image-2-edit.md` | 9:16, 1152×2048 | high | ~$0.16–0.30 |
| 5. UGC video (one only) | Seedance 2.0 Fast (reference-to-video) | `models/seedance-2-fast-reference.md` | 9:16, **720p** | 15 seconds, native audio | ~$3.63 |
| Uploads (utility) | Kie AI file upload | `models/kie-upload.md` | — | — | free |

Prices move. Treat these as ballparks and say so when quoting. Never use lower quality or lower resolution on image steps to save money — the sheets are the identity anchors for the video.

**Why the models differ — do not "simplify" this.** Step 1 is text-only, so it runs on Nano Banana 2, a fast, cheap, photoreal text-to-image model. Steps 2–4 all work by feeding reference images in (the chosen model image, the user's product photos, the sheets themselves), so they **must** run on GPT Image 2's edit endpoint, which accepts reference images and preserves printed label text. Step 5 must run on Seedance 2.0 Fast reference-to-video because it is the only video model here that binds several reference images at once (identity + product + scale). Never route a reference-image step to a text-only endpoint, and never route Step 5 to a single-start-frame video model without the user's explicit opt-in.

**Step 1 is not "draft cheap, finish pretty."** A rerun of the same prompt produces a *different person*, so there's no way to upgrade a cheap draft into a final. Step 1 runs on full Nano Banana 2 from the start. The Lite variant is only for when the user explicitly wants lots of throwaway looks to find a direction (see the recipe).

**720p note (say this to the user at step 5):** "Heads up — I'll render at 720p. Seedance 2.0 Fast on fal tops out at 720p. If you really need 1080p, Kling 3.0 can do it, but it holds your model and product less consistently. Just ask and I'll walk you through it." (See `models/kling-3.md`.)

**Fixed output — one 15-second video:** this pipeline produces exactly **one video, 15 seconds long**. Duration is not a question to ask and not a setting to negotiate. If the user asks for something longer (e.g. "a 40-second video") or for several clips, tell them plainly that this skill delivers a single 15-second video, and plan the concept to land inside that window — tighten the script rather than splitting it. In Manual mode, deliver exactly one prompt.

## Provider routing

1. This skill uses exactly two providers: **fal.ai for all generation, Kie AI for uploads** (and the opt-in Kling route). No other provider is used.
2. If fal.ai lacks the model, fails auth, or returns a server error, **stop and tell the user**: show the error, say what it means in plain words, and ask how to proceed. Retry once only for a clear transient failure (timeout, 5xx) before any paid job was accepted, and say you did. Never switch a step to a different model to get past an error.
3. If the Kie upload fails, the fal-storage upload route in `models/kie-upload.md` is the only fallback. Say which route ran and why.
4. A content-policy rejection is **not** a transient error. Show the user the message and ask.
5. Kling 3.0 (`models/kling-3.md`) is **never** a fallback for Step 5. It's used only when the user asks for it by name, after hearing the consistency warning.
6. If a call returns "model not found", the provider has renamed the model. Check the provider's model page, update the recipe file, and tell the user.

## Output: one folder, flat

- Save every file **flat** into the user's generations folder. Default: `generations/` in the workspace root. If the user or their `CLAUDE.md` names another path, use that. Never create subfolders except `refs/`.
- `generations/refs/` holds the user's own input images (product photos, scale photo). Copy uploads there on receipt so they can be re-uploaded later — Kie URLs expire after 3 days.
- **Naming:** `{project}_{description}_{timestamp}.{ext}`: lowercase, hyphens inside parts, Unix timestamp. Ask for a short project name at the start (default: the product's name, e.g. `glowserum`). Descriptions for this pipeline:
  - `model-v1`, `model-v2`, … (Step 1 variations)
  - `character-sheet`, `product-sheet`, `scale-ref`
  - `ugc-video`
  - e.g. `glowserum_character-sheet_1774912000.png`, `glowserum_ugc-video_1774913500.mp4`
- **Sidecar log:** after every save, write a JSON file beside it with the same basename and a `.json` extension:
  ```json
  {
    "model": "bytedance/seedance-2.0/fast/reference-to-video",
    "provider": "fal.ai",
    "step": 5,
    "prompt": "the full text prompt that was sent to the API",
    "refs": ["glowserum_character-sheet_1774912000.png", "glowserum_product-sheet_1774912300.png", "glowserum_scale-ref_1774912600.png"],
    "params": { "aspect_ratio": "9:16", "resolution": "720p", "duration": "15" },
    "seed": 123456,
    "cost_quoted_usd": 3.63,
    "created": "2026-09-22T09:41:00Z"
  }
  ```
  `refs` lists local filenames (not expiring URLs), in the exact order they were passed. This is how any file can be reproduced later.
- Download results **immediately** — provider result URLs expire.
- Run multiple generations **one at a time** (e.g. Step 1 variations) to avoid rate limits.

## Step gating

Work through the steps strictly in order. Do not start a step until the previous one is complete (generated/delivered AND user-approved). Track where you are; if the user tries to skip ahead (e.g. "just make the video"), explain briefly why the earlier assets matter for consistency and offer the fast path through them. Steps may be repeated as many times as the user wants — only move on when they're happy.

**Returning users:** if the generations folder already holds an approved `character-sheet`, `product-sheet`, and `scale-ref` for this project (check the sidecar logs), offer to jump straight to Step 5 with them. That's the payoff of the pipeline.

**Never describe a face or a product in words when you have the real image.** Pass the real file as a reference. If a needed image is missing, stop and ask for it.

### Inputs map (exactly these, per generation)

1. **Model images** → text prompt only
2. **Character reference sheet** → chosen model image + verbatim character sheet template
3. **Product reference sheet** → user's product images + verbatim product sheet template
4. **Product scale reference** → user's scale photo + character reference sheet image + product reference sheet image + verbatim interaction template
5. **UGC video** → character reference sheet image (`@Image1`) + product reference sheet image (`@Image2`) + product scale reference image (`@Image3`) + the composed video prompt

In API mode, every local image goes through `models/kie-upload.md` to get a public URL first. In Manual mode, spell out this exact attachment list and order to the user at each step so they upload the right images.

---

## Step 1 — Model images

Open warmly, e.g.: "Let's start by creating your model — the AI influencer who'll be the face of your content. Once we nail their look, we keep them consistent across every image and video."

Ask for the four base inputs, with examples: **age** (early 20s, mid 30s…), **gender**, **influencer type** (beauty, fitness, lifestyle, tech, mom-creator, streetwear…), **ethnicity** (Latina, East Asian, Black, Middle Eastern, mixed…). Then invite optional control: eye color, hair style/color, freckles, body type, wardrobe, vibe ("girl-next-door" vs "polished editorial") — the more they give, the closer the model lands to what's in their head.

Once details are in, ask: **"How many variations should I generate — 1, 3–5, or 10?"** Recommend at least 3–5 to compare and find the perfect fit, and reassure them this step can be rerun with tweaks as many times as needed. Quote the cost (variations × ~$0.12).

Generate with **Nano Banana 2** (`models/nano-banana-2.md`), text prompt only — no reference media on this step. To write the prompt(s), read `references/model-image-builder.md` and follow it fully — it is the complete model-image methodology and points to `references/visual-framework.md`, `references/framework.md`, and `references/examples.md`, which you must also read before writing. Honor its base-photo rule (no product in hands), facing rule, and imperfection rule. Each variation is a 9:16 image.

Gate: the user picks one final model image. That image becomes the identity anchor for everything downstream.

## Step 2 — Character reference sheet

Explain the purpose in friendly terms: "Now I'll turn your chosen model into a character reference sheet — 6 angles in one sheet (4 close-ups + 2 full-body). This locks in her identity so she stays the same person in every image and video we make."

Use the prompt in `references/templates/character-reference-sheet.md` **verbatim — word-for-word, never paraphrased, trimmed, or 'improved'** — with the approved model image attached. Generate with **GPT Image 2 edit** (`models/gpt-image-2-edit.md`) — the reference image requires it.

Gate: ask the user to check consistency (same face, hair, outfit across all 6 panels) and approve.

## Step 3 — Product reference sheet

Explain warmly why this matters: when an AI video model generates footage of a product, it needs to see it from every angle — if it can only see the front, it *guesses* the back and sides, which is where hallucinated labels, warped caps, and made-up details come from. The fix: combine the user's product photos into one clean reference sheet, so the model never guesses and the user never has to upload a pile of images per video.

Ask the user to send their product photos: **at least 2–4 recommended, up to 6**, from different angles — front, back, sides, top, detail shots of labels or caps. The more angles, the more faithful the product stays. Save them into `generations/refs/`.

Note to the user: this sheet covers the product's *look*, not its *scale* — that's the next step.

Use the prompt in `references/templates/product-reference-sheet.md` **verbatim** with their product images attached. **GPT Image 2 edit** (`models/gpt-image-2-edit.md`).

Gate: user approves the sheet (product faithful, nothing invented).

## Step 4 — Product scale reference (optional, highly recommended)

Present as optional but strongly recommended, and explain why: the reference sheet shows what the product looks like, but not how **big** it is. Without a scale anchor the video model guesses the size — wrongly, and *differently across generations* (palm-sized serum in one video, forearm-sized in the next). The fix: ask the user to upload a photo of the product **next to something universally sized** — held in a hand is perfect; a phone or coin also works. That comparison teaches the model the true scale. Save it into `generations/refs/`.

Then generate the interaction image using `references/templates/character-product-interaction.md` **verbatim**, attaching, in this order: the user's scale photo + the character reference sheet + the product reference sheet. **GPT Image 2 edit** (`models/gpt-image-2-edit.md`), 9:16. This image locks correct scale into every video.

If the user skips: note the higher risk of inconsistent sizing and proceed to Step 5 (the video then uses just the character and product sheets, `@Image1` and `@Image2`).

Gate: user approves (product fully visible, label facing camera, scale realistic) — or explicitly skips.

## Location handling (before video generation)

**Composition principle — why locations are separate:** giving the video model a single image with the model already placed inside the scene makes the result feel AI-generated, like it's just animating a still. Providing the location and the model as *separate* references lets the video model compose the scene itself — more real, less flat, better depth and sharpness. So never pre-compose model-in-scene images for video input; pass references separately.

**No location image is generated.** A separate location reference earns its cost only when several generations must share one identical environment. Since this pipeline outputs a single video, there is nothing to keep consistent across — so skip the location image entirely and **describe the setting inside the video prompt instead**.

Still ask the user to describe or confirm the setting (and suggest one that fits the product if they have nothing in mind) — it just goes into the prompt as words rather than a generated reference image. Then move straight to generation.

## Step 5 — UGC video (one, 15 seconds)

Before composing, read `references/video-prompt-builder.md` and follow it **100%** — format fork, technical header, scene/creator behavior rules, camera-rig logic, timestamped choreography, dialogue density, the accent question, negative constraints, deterministic no-"or" rule, and the final checklist. That document is the law for video prompts. Apply these overrides on top:

- Engine: **Seedance 2.0 Fast reference-to-video** (`models/seedance-2-fast-reference.md`), 9:16, **720p** — deliver the 720p note above.
- **Exactly one 15-second clip** — never longer, never split into several. Write the choreography to fill 15 seconds with roughly three to four short spoken lines.
- Reference inputs per the inputs map: character sheet + product sheet + scale reference — always as separate references, never a pre-composed scene.
- **Bind every asset explicitly in the prompt** with `@Image1` (character sheet — identity and outfit), `@Image2` (product sheet — the product), `@Image3` (scale reference — product size), and declare the assets the source of truth. The label numbers must match the order of `image_urls` (API mode) or the attach order (Manual mode). Full method in `references/video-prompt-builder.md` (Step 5.5) — read it before composing.
- **Close with the realism block** — positive "looks like real iPhone footage" statement, then native-audio sound direction (voice + room tone), then the negatives including the mandatory crew-invisibility clause. Seedance generates its own audio (`generate_audio: true`), so never leave sound unspecified.

Ask the planning questions conversationally: what's the video about, continuous take vs jump cuts, and setting. Do **not** ask about duration — it's fixed at 15 seconds; just mention it in passing so they know the runtime they're writing for. Then ask the three **creator delivery inputs** together as a natural group — frame it like: "Now let's nail how she sounds and feels on camera:"

- **Accent** — which accent does the creator speak in? *(default: neutral American — e.g. British, Australian, Nigerian, Spanish, etc.)*
- **Energy** — what's the vibe on camera? *(e.g. hype and high-energy, calm and authoritative, bubbly and fun, soft and conversational, warm and relatable)*
- **Speaking pace** — how fast or slow does she talk? *(e.g. fast and punchy, slow and deliberate, natural conversational pace, excited and quick)*

All three get written into the creator's delivery description in the prompt and included in the pre-generation summary. Default to neutral American accent, natural conversational pace, and warm/relatable energy if the user doesn't specify.

Then — in API mode — present the **pre-generation summary** (scene, beats, full dialogue, accent, energy, pace, 15 seconds, 720p, model, ~$3.63) and get explicit approval before submitting. The job runs on the fal queue and takes a few minutes. Tell the user it's rendering, poll patiently, and never resubmit because it feels slow. After the clip, save it and its sidecar log, then do the post-generation check. In Manual mode, deliver the single full prompt plus the exact list of images to attach, in order, and the settings (15s, 720p, 9:16, audio on).

After the approved video, congratulate them — and offer to run the step again for a fresh 15-second video: a new script, a new setting, a new angle, all reusing the same locked assets (that's the payoff of the pipeline: steps 1–4 never need redoing for the same model + product).

## Reference files

- `references/model-image-builder.md` — full Step 1 methodology (read before writing model prompts)
- `references/visual-framework.md`, `references/framework.md`, `references/examples.md` — the model-image visual vault, layered prose framework, and worked examples (read all three before writing model prompts; examples are inspiration only, never copy)
- `references/video-prompt-builder.md` — full Step 5 methodology (read before writing any video prompt)
- `references/templates/character-reference-sheet.md` — Step 2 verbatim template
- `references/templates/product-reference-sheet.md` — Step 3 verbatim template
- `references/templates/character-product-interaction.md` — Step 4 verbatim template

## Model recipes

One file per model. When a better model ships, add or edit one recipe file and update the settings table above. Nothing else changes.

- `models/nano-banana-2.md` — Step 1
- `models/gpt-image-2-edit.md` — Steps 2–4
- `models/seedance-2-fast-reference.md` — Step 5
- `models/kie-upload.md` — local file → public URL
- `models/kling-3.md` — opt-in alternate video engine (never automatic)
