# UGC Prompt Framework — Layered Structure

## How to use this alongside the Visual Framework

Before writing a single word of prose, make your visual decisions using the vocabulary in `visual-framework.md`. Think of yourself as a Visual Director walking onto a set — every choice is intentional.

The process is two steps:

**Step 1 — Decide** (internal, using the vault)
Run through the six elements and pick your choices:
- **Subject** — skin texture, expression, micro-details, avoids
- **Framing** — shot size, angle, lens type, composition, camera feel
- **Lighting** — source, direction, quality, color, effects
- **Mood** — emotional state, tone/energy, color grade, environment
- **Medium** — always Smartphone (iPhone) for UGC; add subtype and avoids
- **Style** — photography style (UGC, Social Media Raw, Hyperreal, etc.)

**Step 2 — Write** (prose output)
Translate those decisions into the layered prose structure below. The vocabulary drives the precision; the prose is what the user gets. Don't list the framework elements — absorb them and write naturally.

The output is always flowing paragraphs, never a structured breakdown.

---

Build the final prompt as flowing prose paragraphs using these layers in order. Each layer adds a dimension of believability. The goal is a shot that reads as a real iPhone photo — not a studio render.

---

## Layer 1 — Opening line: medium + scene + subject

Open with one sentence that immediately sets the photographic medium, scene, and subject. This is the frame everything else hangs off. Use "hyper-realistic vertical smartphone [shot type]" to anchor the iPhone aesthetic from the start.

> "Create a hyper-realistic vertical smartphone selfie portrait of a young woman seated indoors at a restaurant table..."
> "A hyperreal smartphone iPhone UGC photo of a Brazilian-Mediterranean female dance influencer in her mid-20s inside a moody dance studio..."
> "Create a hyper-realistic vertical smartphone beachside portrait of a young woman leaning against a white beach-club railing near the ocean at golden hour..."

---

## Layer 2 — Camera POV & angle

Describe the exact camera position, angle, and shooting scenario. There are only two valid shot types for these base photos — a front-facing selfie, or a waist-up photo taken by someone else. **Never a mirror selfie** (no rear-camera-into-a-mirror, no visible mirror frame, no phone held up to a reflection).

**Always front-on.** In both shot types the model faces the camera directly — body squared to the lens, the full front of the figure visible from waist to head, with a direct gaze. The camera angle may sit slightly above or below eye level, but the *figure* is never in profile, never turned three-quarters away, never angled off-camera.

- **Selfie (front-facing)**: "captured extremely close from slightly above eye level with a front-facing phone camera perspective, arm's-length, body squared to the lens"
- **Photo taken by someone else (waist-up)**: "photographed waist-up from a slightly low angle, as if someone else is taking the photo on an iPhone, the model facing the camera directly with the full front of the figure visible from waist to head"

---

## Layer 3 — Subject position & pose

Describe exactly where the subject is in space and what her body is doing. Hands, arms, posture, and weight placement all matter for realism. Vague posing produces stiff AI poses — specificity produces candid.

**Base-photo rule — no held products.** This is the model's clean base image; a product gets composited in later. The subject must never hold, present, or grip any product or object (no serum, bottle, can, cup, drink, food, phone-held-up, or branded item). Keep hands free and natural — resting on a surface, on a railing, in a pocket, lifting or tucking hair, framing the face, adjusting a collar or earring. Lifestyle items may exist in the environment (a glass on the table, a cup on the counter) but stay out of the hands.

**Facing rule — always front-on.** The body is squared to the camera with the full front of the figure visible from waist to head, and the gaze goes directly into the lens. Never pose the model in profile, turned away, or angled three-quarters off-camera. Hands and small head tilts are fine, but the torso stays front-facing so the whole front of the model reads clearly.

Include:
- Body angle and orientation (facing camera, three-quarter, turned away)
- What each arm/hand is doing (resting on table, lifting hair, on railing, crossing frame)
- Head position and neck angle
- Specific body contact points (elbow on table, hand on face, fingers near hair)

> "Her left cheek rests against her left hand, fingers lightly curled beside her face, elbow on the table. Her other forearm crosses the bottom of the frame."
> "Her body is turned slightly away from the camera in a three-quarter profile, but her head turns back toward the lens with direct eye contact. One arm rests casually on the railing, while the other hand lifts toward her slicked-back hair."

---

## Layer 4 — Physical identity

Give the model precise, specific physical features. Generic descriptors produce generic results — specificity produces a believable individual.

- **Eyes**: color + quality ("pale blue-green eyes with sharp catchlights", "warm honey-brown eyes with visible iris rings")
- **Brows**: thickness + style ("thick dark eyebrows", "soft natural blonde brows")
- **Facial structure**: cheekbones, nose, lips ("defined cheekbones, straight nose, full lips with a neutral pout")
- **Lashes**: length and density without over-styling ("long lashes", "natural lashes with barely-there mascara")
- **Expression**: precise emotional register ("serious, confident, slightly bored, editorial, and intimidating" — never just "beautiful" or "confident")
- **Hair**: color, texture, styling — be exact ("slicked-back blonde hair pulled tightly into a wet-look bun, with darker roots and subtle golden highlights")
- **Skin tone**: warm descriptor ("deeply sun-kissed warm bronze", "olive-tan with natural glow", "rich deep brown with warm undertones")

### Authenticity & asymmetry (required)

Generators default to flawless, perfectly symmetrical faces — which is exactly what makes outputs read as AI. Negating perfection ("no plastic skin") is not enough; you must **positively describe real human imperfections**. Every model gets **2–3 specific, subtle asymmetries or flaws** woven into the identity description. Name them concretely — vague "imperfect features" does nothing.

Pull from, and vary across, these:
- **Brows**: slightly uneven — one a touch higher, thicker, or with a small gap or stray hairs ("naturally uneven brows, the left sitting slightly higher")
- **Eyes**: faint asymmetry — one marginally smaller, a slightly heavier eyelid on one side, mild under-eye shadow
- **Lips / smile**: a faintly asymmetric mouth, one corner lifting more, a slightly fuller lower lip on one side
- **Nose**: a subtle bump, a faint deviation, slightly asymmetric nostrils
- **Marks**: a small mole, a beauty mark, a freckle cluster, a faint old scar, a tiny blemish
- **Teeth** (if visible): natural, not veneered — a slight overlap or gap

> "Her brows are naturally uneven — the right slightly higher and a touch fuller — and her smile pulls a little more to one side. A small mole sits below her left eye, and her lower lip is faintly fuller on one side."

**Subtlety guardrail:** keep these believable and human — 2–3 gentle cues, never deforming. The goal is "a real person, not a model render," not a warped or distorted face.

---

## Layer 5 — Skin realism

This is the believability layer. Always include it. Real iPhone photos don't smooth skin — they show pores, peach fuzz, sheen, and imperfections. The more specific, the more real.

Go beyond texture into **tone irregularity** — real skin is never one uniform color. Work in a couple of: faint redness around the nose or on the cheeks, a small blemish or two, slightly uneven skin tone, a faint vein at the temple, mild under-eye shadow or darkness, slightly chapped or uneven lips, a faint tan line. These pair with the asymmetry cues from Layer 4 to break the "retouched magazine skin" look.

Tailor to context:

**Sun / outdoor / warm:**
> "Skin has realistic pores, soft sheen from sunscreen and sea humidity, fine facial texture, natural small marks and unevenness. Realistic glossy golden-hour highlights on shoulders, collarbones, arms, and face."

**Studio / sweat / athletic:**
> "Visible pores, sweat beads, fine baby hairs, tiny freckles, natural body texture, collarbone sheen. No beauty smoothing or airbrushing."

**Indoor / evening / candid:**
> "Realistic pores, soft sheen on forehead, nose, cheeks, shoulders, and arms, with natural texture and subtle unevenness, faint redness around the nose, and a small blemish near the chin. No plastic skin, no over-smoothing."

Always end with: **"No plastic skin, no over-smoothing."** or **"Absolutely no beauty smoothing or airbrushing."**

---

## Layer 6 — Outfit & accessories

Be specific about fabric, color, fit, and details. Jewelry deserves its own sentence — describe exactly what's worn and how it sits in the light.

> "She wears a black satin camisole with cream lace trim around the neckline and straps, a thin gold necklace, a gold ring, and two gold watches/bracelets visible: one on the wrist near her face and another near the bottom of the frame. The jewelry catches warm highlights but should not look overly polished."

Note: if there are multiple accessories, describe where each one sits in the frame — this prevents the model from duplicating or warping them.

---

## Layer 7 — Environment & background

Describe the background with enough detail that it reads recognizably, but specify it as blurred so it doesn't compete. Name specific background elements (furniture, walls, people, objects) and their position in frame.

> "The environment is a warm upscale restaurant interior at night. Behind her, softly blurred background with a second seated woman on the left looking down at a phone, plates and tableware barely visible, a textured beige wall with branch-like decor on the left, and a dark reddish-brown vertical wooden panel wall on the right. Background should be shallow depth of field, naturally blurred, but still recognizable."

> "Softly blurred ocean horizon behind her, warm sand tones, pale blue water, golden sunset sky, linen cabanas or cream umbrellas, wooden deck flooring, and faint silhouettes of beach-club guests in the distance."

---

## Layer 8 — Lighting

Describe the light source, its direction, quality, and what it does to the subject's face and body. Avoid generic terms like "good lighting" — name the source and the effect.

**Warm restaurant / indoor:**
> "Lighting is warm, dim, intimate restaurant lighting with strong overhead/front warm illumination on the subject's face and shoulders, creating glossy highlights and soft shadows under the brow, nose, lips, and hand."

**Golden hour outdoor:**
> "Natural golden-hour sunlight from the side/front, creating warm highlights along her cheekbones, forehead, shoulders, collarbones, and arms. Gentle shadows define the nose, jawline, neck, and hand."

**Studio / overhead:**
> "Hard warm overhead studio light, soft mirror reflections, and specular sweat highlights."

---

## Layer 9 — Technical & composition

Describe the crop and iPhone-specific qualities that sell the shot as a real phone photo.

Always include:
- Crop and what's in frame
- Phone-camera characteristics that make it feel real

> "Composition: close crop, face near the upper center, shoulders and neckline visible, left hand framing the face, forearm cutting diagonally across the lower frame. Shot on iPhone with natural lens compression, slight digital noise, true-to-life color science, no post-processing filters, and no studio lighting artifacts."

> "Vertical 9:16 crop, subject centered with slight negative space to the right. iPhone front-camera proximity distortion slightly visible — nose and face fractionally larger than background elements. Natural sensor noise in shadows, no sharpening, no beauty mode."

> "Medium close-up, head and upper chest filling the frame, slight off-center composition. iPhone rear camera with portrait mode off — everything in focus with natural depth falloff. Candid handheld feel with micro camera shake."

Always end with a phrase like: **"Shot on iPhone, no filters, no studio polish, no AI smoothing artifacts."**

---

## Negative Prompt

Close every UGC prompt with a single negative prompt line. Combine all relevant Avoid items from the Visual Framework into one line. Use this as your base and adapt as needed:

> **Negative prompt:** No beauty mode, no skin smoothing, no airbrushing, no filters, no plastic skin, no perfect symmetry, no flawless idealized features, no retouched magazine skin, no doll-like face, no forced smile, no stiff pose, no lens distortion, no vignette, no over-cropping, no blown highlights, no flat lighting, no harsh flash, no overexposure, no generic mood, no oversaturated grade, no watermark, no stock photo look, no text overlays, no border / frame, no CGI look, no plastic surfaces, no clean compositing, no video game render, no AI smoothing, no neon fantasy tropes, no cartoon style.

Note: the anti-perfection terms above (`no perfect symmetry`, `no flawless idealized features`, `no retouched magazine skin`, `no doll-like face`) only work in tandem with **positively described** imperfections in Layers 4 and 5. Negation alone won't add asymmetry — you must also describe the real flaws.
