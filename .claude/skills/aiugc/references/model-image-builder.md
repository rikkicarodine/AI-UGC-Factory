
# UGC Base Model Image Prompt Builder

This skill produces ready-to-paste AI image prompts for hyper-realistic UGC model shots — candid lifestyle portraits, selfies, editorial beauty, and scene-based content that looks indistinguishable from real iPhone photography.

These prompts are generated with **Nano Banana 2** (see `models/nano-banana-2.md`) from a **text prompt only**. No reference images go in on this step, so everything about the model's identity must live in the prompt text itself. That makes the layered description below the whole game: there is no reference photo to fall back on.

## Step 1 — Build the model

Ask these four questions conversationally, one natural flow. Make it feel like a quick creative conversation, not a form. Lead with:

> "Let's build your model. Just answer what feels relevant — skip anything you don't care about and I'll fill it in."

Then ask:

1. **Gender / presentation** — Female, male, or non-binary?

2. **Ethnicity or background** — Any specific heritage, a broad region, or no preference?
   *e.g. Brazilian, West African, South Korean, Middle Eastern, mixed Mediterranean, no preference*

3. **Age range** — How old do they look?
   *e.g. early 20s, late 20s, 30s, 40s, mature creator, no preference*

4. **Influencer niche or style** — What world do they live in?
   *e.g. beauty, fitness, fashion, wellness, luxury lifestyle, tech, mom creator, finance, gaming, travel, food, streetwear, clean-girl*

Then close with the optional extras line:

> "Optional extras: hair, wardrobe, natural background or location, signature feature, or any specific vibe you want."

If they skip everything, jump straight to Step 2 with strong creative defaults and offer to adjust after.

> **Base-photo rule:** This output is the model's *base* image — a clean foundation that a product will be composited into later. The model must NEVER hold, present, or pose with any product (no beauty serum, bottle, can, phone-held-up, drink, food, or branded item). Hands stay free and natural — resting, on a railing, in a pocket, lifting hair, framing the face. Niche or brand category can inform wardrobe, setting, and styling, but never puts an object in their hands.

> **Facing rule:** The model always faces the camera directly — body squared to the lens with the full front of the figure visible from waist to head, and a direct gaze into the camera. Never a side/profile view, never turned three-quarters away, never with the body angled off-camera. The camera may sit slightly above or below eye level, but the figure itself stays front-on.

> **Imperfection rule:** Perfectly symmetrical, flawless faces are the #1 AI tell. Every model must include 2–3 subtle, positively described real-world imperfections — uneven brows, faint feature asymmetry, a small mole or blemish, tone unevenness, slightly chapped lips, imperfect teeth. Negating perfection in the negative prompt is not enough; the flaws must be written into the description. Keep them believable and human, never deforming. See Layers 4 and 5 of `framework.md`.

## Step 2 — Gather the shot

Ask the user the four questions below. Keep the same conversational tone:

> "Now let's set the shot."

1. **Scene / setting** — Where are they, and roughly what time of day?
   *e.g. rooftop bar at night · beach club golden hour · gym mid-morning · café window seat · hotel bathroom pre-night-out · outdoor market midday*

2. **Shot type** — How is the photo being taken? Only two options:
   *selfie (front-facing, close, arm's-length) · or a photo taken by someone else (waist-up)*
   Never a mirror selfie. If they ask for a mirror shot, steer them to one of these two instead.

3. **Vibe / mood** — What feeling should it give off?
   *e.g. serious editorial · warm and candid · post-workout intensity · dark and moody · luxury lifestyle · clean and minimal*

4. **Pose or action** — Anything specific they're doing, or leave it open?
   *e.g. leaning on railing, lifting coffee, looking off-camera, mid-laugh*

If they give you nothing at all, proceed with strong creative defaults.

## Step 3 — Build the prompt

Before writing the prompt, read all three:
- `references/visual-framework.md` — the visual vocabulary vault (Subject, Framing, Lighting, Mood, Medium, Style). Use this to make intentional visual decisions before writing a single word.
- `references/framework.md` — the layered prose framework, UGC shot examples, and negative prompt guidance. This explains how to translate your visual decisions into the final prompt.
- `references/examples.md` — a library of complete worked prompts across many models, ethnicities, scenes, and shot types. These are **inspiration only** — use them to calibrate density, tone, and structure, and to see how the layers come together. Never copy or lightly reskin one. Always write a fresh, original prompt for the user's specific request.

Combine everything from Step 1 and Step 2 into the layered prose structure. The model identity built in Step 1 feeds directly into Layers 3, 4, and 5 of the framework. The process: decide first using the vault, then write in prose.

## Step 4 — Deliver

Output the prompt as clean prose paragraphs followed by a single negative prompt line. No headers, no bullets inside the prompt itself — just raw text the user can paste directly.

After the prompt, add 1–2 brief notes on easy tweaks.
