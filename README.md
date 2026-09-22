# AI UGC Factory — `/aiugc`

A Claude Code skill that builds a 15-second AI UGC video from scratch:

**model image → character reference sheet → product reference sheet → product scale reference → one 15s selfie-style UGC video**

Each step's output feeds the next, so the face, outfit, and product stay the same in every video. Once Steps 1–4 are done for a model and product, new videos only need Step 5.

It's a remix of two sources:

- **The prompt craft** comes from the `higgsfield-mcp-ugc-builder` skill: the layered prompt framework for the model image, the three word-for-word reference-sheet templates, and the video rules for timestamped beats, camera setup, dialogue, and realism. That material is kept as is.
- **How it runs** comes from the RoboNuggets *"Build your own /generate skill"* guide: calling the models directly on fal.ai and Kie AI instead of through Higgsfield, one settings file per model, a price quote before every paid run, one flat output folder, and a JSON log beside every file.

## Models

| Step | Model | Provider | Approx. cost |
|---|---|---|---|
| 1. Model image (text only) | Nano Banana 2 · 9:16 · 2K | fal.ai | ~$0.12 / image |
| 2. Character sheet | GPT Image 2 edit · 16:9 · high | fal.ai | ~$0.16–0.30 |
| 3. Product sheet | GPT Image 2 edit · 16:9 · high | fal.ai | ~$0.16–0.30 |
| 4. Scale reference | GPT Image 2 edit · 9:16 · high | fal.ai | ~$0.16–0.30 |
| 5. UGC video | Seedance 2.0 Fast reference-to-video · 9:16 · 720p · 15s · audio | fal.ai | ~$3.63 |
| Uploads | Kie AI file upload (local image → public URL) | Kie AI | free |

A full first run costs about **$5**. Each extra video with the same model and product costs about **$3.63**. Prices change often, so check the providers' pricing pages.

## Install

Copy `.claude/skills/aiugc/` into your project's `.claude/skills/` (or `~/.claude/skills/` to use it everywhere), or download `dist/aiugc.zip` and unzip it there.

Then add your keys:

```bash
cp .env.example .env   # then fill in FAL_KEY and KIE_API_KEY
```

- `FAL_KEY` — from fal.ai. Required for all generation.
- `KIE_API_KEY` — from kie.ai. Required to upload your product photos.
- `WAVESPEED_API_KEY` — optional backup provider.

Without keys, the skill runs in **Manual mode**: it walks the same pipeline and hands you ready-to-paste prompts, settings, and the exact images to attach at each step.

## Use

Type `/aiugc` in Claude Code, or just ask for "a UGC video for my product".

Outputs are saved flat into `generations/`, named `{project}_{description}_{timestamp}.{ext}`, each with a `.json` log beside it. Your own input photos go in `generations/refs/`. The `generations/` folder is git-ignored.

## Layout

```
.claude/skills/aiugc/
├── SKILL.md                         pipeline, gates, routing, output rules
├── models/                          one settings file per model
│   ├── nano-banana-2.md             Step 1
│   ├── gpt-image-2-edit.md          Steps 2–4
│   ├── seedance-2-fast-reference.md Step 5
│   ├── kie-upload.md                upload utility
│   └── kling-3.md                   opt-in alternate video engine
└── references/                      prompt craft (from the original skill)
    ├── model-image-builder.md
    ├── visual-framework.md
    ├── framework.md
    ├── examples.md
    ├── video-prompt-builder.md
    └── templates/                   verbatim — never edit
        ├── character-reference-sheet.md
        ├── product-reference-sheet.md
        └── character-product-interaction.md
```

## What changed from the original skill

- **Higgsfield removed.** No Higgsfield tools, model names (`soul_2`, `gpt_image_2`, `seedance_2_0`), or `<<<element_id>>>` reference elements.
- **Step 1** now runs on Nano Banana 2 instead of Soul 2.0. It still uses a text prompt only.
- **Video image labels.** The video prompt refers to its images as `@Image1` (character sheet), `@Image2` (product sheet), and `@Image3` (scale reference) in both modes. Seedance reads these labels directly. The original's example label order didn't match the images the pipeline actually passes, so it was corrected.
- **Key check.** The opening "Is the Higgsfield tool connected?" check became "Are the API keys set?", with API mode and Manual mode.
- **New from the guide:** a price quote before every paid step (image steps included), provider backups that are always announced, settings files per model, a flat output folder, and a JSON log beside every file.
- **Unchanged:** the model-image method, the visual vocabulary, the prose framework, the examples, all three templates, the video beat, camera, dialogue, and realism rules, the approval before and after every generation, and the single 15-second video.

## Checked against live docs (2026-09-22) and still open

Checked:

- **Seedance 2.0 Fast:** model name, `image_urls` (up to 9), the `@ImageN` syntax, 4–15s length, 480p/720p only, and $0.2419/s.
- **GPT Image 2 edit:** `image_urls` (up to 16), custom `image_size` rules, and `quality`.
- **Nano Banana 2:** model name `fal-ai/nano-banana-2`, `resolution` 0.5K–4K, and `aspect_ratio`.
- **Kie AI upload:** endpoint `kieai.redpandaai.co/api/file-stream-upload` and the response field `data.downloadUrl`.

Still open:

- [ ] **No 1080p on the main video model.** Seedance 2.0 Fast on fal tops out at 720p. The skill says so, and lists two other routes if you ask: WaveSpeed's Seedance tiers, and Kling 3.0 `pro`.
- [ ] **WaveSpeed isn't confirmed.** Nobody has checked yet that WaveSpeed's Seedance variant accepts several reference images. Until then it's only a backup.
- [ ] **Kling 3.0 fields aren't confirmed.** Its request fields on Kie (`models/kling-3.md`) follow the guide's pattern but haven't been checked one by one.
- [ ] **GPT Image 2 prices are estimates.** The exact per-image price at 2048×1152 high quality is estimated from fal's published size tiers.
