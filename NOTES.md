# AI UGC Factory — merged skill (notes)

## What this is

A merge of two skills into one: `ai-ugc-factory.zip` (delivered to Rikki 2026-09-22).

- **Source A** — a "done-for-you" 5-step UGC pipeline skill (`higgsfield-mcp-ugc-builder`): model image → character reference sheet → product reference sheet → product scale reference → 15s UGC video. Strong prompt-engineering content (layered prose framework, verbatim reference-sheet templates, timestamped video-prompt choreography rules). Originally wired to the Higgsfield MCP.
- **Source B** — a RoboNuggets PDF guide ("Build your own /generate skill") describing a provider-agnostic routing pattern across Kie AI (kie.ai) and fal.ai, with model ids, auth patterns, cost ballparks, and folder/logging conventions.

## What changed

- Removed all Higgsfield MCP dependencies (tool calls, `soul_2`/`gpt_image_2`/`seedance_2_0` ids, `<<<element_id>>>` reference-element mechanic).
- Kept ~90% of the original content unchanged: the prompt-writing framework, the three verbatim reference-sheet templates, and the video choreography/realism rules are all provider-agnostic and untouched.
- New model routing:
  - **Step 1** (model image, text-only) → Nano Banana 2 (`gemini-3.1-flash-image-preview` / Lite) via fal.ai
  - **Steps 2–4** (reference-image edits/composites) → GPT Image 2 (`openai/gpt-image-2`, `/edit`) via fal.ai
  - **Step 5** (15s UGC video, multi-reference) → Seedance 2.0 Fast (`bytedance/seedance-2.0/fast/reference-to-video`) via fal.ai — chosen because it's the only one of the guide's three video options that accepts multiple bound reference images (needed to lock identity + outfit + product simultaneously), matching the architecture the original pipeline depended on.
  - **Kie AI's role:** file-upload utility (local image → public URL, required by fal.ai's reference fields), plus an optional cheaper fallback video engine (Kling 3.0) for non-hero shots, explicitly flagged as lower consistency since it only takes a single start frame.
- Added from the RoboNuggets guide: cost-quote gating before every paid run, flat output folder + `refs/` subfolder convention, `{project}_{description}_{timestamp}` naming, and an optional JSON sidecar log per generated file.
- Replaced the old "Higgsfield MCP available?" branch with an **API mode** (`FAL_KEY` / `KIE_API_KEY` present) vs **Manual mode** (keys missing — deliver prompts + exact upload steps) check.

## Delivery format

Rikki chose a downloadable skill folder/zip (not the single-file claude.ai account-skill proposal card). This preserves the multi-file structure (`SKILL.md` + `references/` + `models/`) for use in Claude Code or any agent that reads folder-based skills, matching the format of the original uploaded skill.

## Known follow-ups / verify before first real run

- [ ] Exact fal.ai model slugs (they mirror provider ids under their own naming; e.g. Nano Banana 2's fal slug wasn't confirmed against live docs) — flagged inline in `models/nano-banana-2.md`.
- [ ] Kie AI's file-upload endpoint path/field name — not in the source PDF, flagged as a TODO to confirm against docs.kie.ai in `models/kie-upload.md`.
- [ ] Whether Seedance 2.0 Fast exposes 1080p (the guide didn't confirm either way).
