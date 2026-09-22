
# UGC Video Prompt Builder

## What this skill is for

AI video models (Veo, Kling, Seedance 2, and similar) take a text prompt plus optional
reference images (a product, an avatar/model) and generate a short clip. The goal
here is a prompt that produces a video indistinguishable from a real creator's
phone footage — natural, slightly imperfect, intimate — not a glossy commercial.

The single most important mental shift: **the reference images carry appearance;
the prompt carries behavior.** If the user passes a product image, do not describe
what the product looks like — describe what the person *does* with it. If they pass
a model image, do not describe her looks — describe her energy and delivery. Adding
appearance description fights the reference image and makes the result worse.

## The final prompt must be deterministic — no "or"

The video model reads the prompt literally; it cannot make choices. So the final
prompt must never contain an unresolved option — no "a gym **or** a home corner," no
"a tumbler **or** bottle," no "British **or** American accent." Every such fork is a
decision the **user** should make, not something you hedge into the text.

Whenever you hit a genuine choice you can't confidently infer from the product, the
images, or context, **ask the user** before generating ("Should this be set in a gym
or at home?") and write only the chosen option into the prompt. The single exception
is the accent question in Step 6.5, which you always ask. Inference is fine when one
option is clearly right (silky loungewear → a bedroom); asking is for real toss-ups.
The output should read as one committed, concrete scene — never a menu.

## The build workflow

Work through these steps in order. Steps 1–2 are near-defaults you confirm; steps
3–6 are where the real work is. Ask the user only for what you genuinely can't
infer from the product, the images, and context.

### Step 1 — Choose the format fork

Every UGC video is timestamped (see Step 5), so the fork is **not** "prose vs.
timecode." The real choice is:

- **Continuous take** — one unbroken shot, no cuts. Intimate, real, best for a
  single product moment. The catch: the camera rig can't change mid-shot (she
  can't set the phone down without it becoming a cut), so every beat must work on
  one rig.
- **Jump cuts** — multiple takes stitched together (HOOK / JUMP CUT / OUTRO, or
  Scene 1, Scene 2…). Needed when the choreography requires different rigs or
  locations, or when you want fast montage energy.

Default to a continuous take for short single-product reviews. Switch to jump cuts
when the action can't physically happen in one shot (see Step 4).

### Step 2 — Write the technical header (always present)

This block is what makes the output read as genuine UGC rather than an ad. It is
**non-negotiable — include it every time.** Assemble it from these slots; the
values in parentheses are the common, safe defaults:

- Aspect ratio + orientation (`Vertical 9:16`; sometimes `3:4`)
- Capture device + camera (`shot on iPhone front camera`, `front and back camera
  mix`, or `filmed on a smartphone`)
- Lighting — **scene-driven, no default warmth.** Name the actual source and let the
  scene decide its character: `natural daylight from a window`, `overcast daylight`,
  `overhead kitchen lighting`, `natural HDR with slight exposure shifts`, `warm
  natural indoor light` (only for a genuinely warm room). Describe the *source and
  direction*, never a glow applied to the creator — no "golden light on her face,"
  no "sun-kissed sheen," no "warm glow across her skin." Skin appearance belongs to
  the attached character reference, and lighting language that acts on her face
  fights it. Light the room; let the reference carry the person.
- Camera handling (`handheld with slight natural movement, casual framing` — but
  see the rig rule in Step 4, which can override this)
- Energy/vibe (`authentic UGC creator energy`; pick the mood: `intimate low-key`,
  `soft cozy`, `fun and expressive`, etc.)
- Realism markers (`real skin tones, no filters, raw, unpolished`)

### Step 3 — Set the scene and the creator

**Scene:** location + light direction + background + mood, in one sentence. Keep the
light neutral unless the user chose otherwise — reaching for "warm" out of habit gives
every video the same cozy grade regardless of product or setting. Let the
model's outfit and the product hint the setting (silky loungewear → soft bedroom /
vanity; sportswear → court; tailored looks → street).

**Creator — behavior only by default.** Describe energy, delivery, and camera
relationship: e.g. *"calm and a little awe-struck, soft-spoken like she's showing a
close friend, looking directly into the lens."* Do **not** describe her appearance —
the avatar image carries that.

- **Wardrobe is a conditional slot.** Omit it by default and let the reference image
  carry the outfit. Only describe wardrobe when the user explicitly wants a
  *different* outfit than the reference. Same principle as the product: specify only
  what you want to change from the image.

### Step 4 — Choose the camera rig (rig follows action)

This is the step most people skip, and skipping it produces physically impossible
footage (e.g. a person holding the phone in one hand while opening a clasp with two
hands — who's holding the phone?). Decide the rig from what the choreography
requires:

- **Handheld selfie (one hand on the phone).** Keeps natural handheld drift. Costs
  one hand, so the product can only be handled one-handed (hold up, tilt,
  single-finger tap).
- **Propped / mounted (phone resting on something, no hands on it).** Frees both
  hands for two-handed actions — opening a clasp, lacing shoes, swinging a bag onto
  the shoulder. The reference prompts do this with lines like *"she props the phone
  against her bag."* When you use this rig, **state the setup explicitly and early**:
  *where the phone sits* (propped on a vanity against a stack of books, leaned on a
  shelf, against her bag on the court), *what it frames* (e.g. "framing her from the
  chest up"), and that **the subject moves toward and away from a fixed lens — the
  camera does not move.** A buried "propped on a vanity" is not enough; the model
  needs to know the camera is locked off and that closeness comes from her leaning
  in, not from a push-in.
- **Back camera handheld.** For filming the product or her feet away from her body.

Two rules that fall out of this:

1. **In a continuous take, commit to one rig that supports every beat.** If a beat
   needs two hands and another needs the intimate handheld feel, that's no longer
   one shot — it forces the jump-cut fork.
2. **A mounted/propped phone is stationary, so the frame is locked — no zoom in or
   out, no push-ins.** A zoom on a propped phone is fake and breaks the "I just set
   my phone down" realism. Handheld may have natural drift, but still no zoom.

**Intimacy is independent of the rig.** Propping the phone does *not* cost intimacy
— that comes from delivery, eye contact, and a soft conversational tone, all of
which work on any rig. When propped, still direct her to *"look directly into the
lens, talking softly like to a close friend"* so the closeness survives.

### Step 5 — Choreograph: pair every beat to an action and a camera move

This is the craft of the whole skill. A UGC prompt is a sequence of **timestamped
beats**, and each beat binds three things together:

> **(time range)** physical action + camera move + spoken line

Timestamp every beat even in a continuous take — it's a pacing control. It tells the
model how long to dwell on each moment, so the hero action gets room to land instead
of being rushed or dragged.

**Name each beat.** Give every beat a one-word function label before its action —
`Opener`, `Use`, `Result`, `Verdict` for a review; `Hook`, `Reveal`, `Proof`,
`Sign-off` for a demo. The label is not decoration: it forces each beat to do a
distinct job, and it makes a weak prompt obvious at a glance (two beats labelled
`Use` means one is redundant). Write it as `(0–4s) Opener: …`.

**Match the line to the move.** The product's best "show" moments come from how she
handles it, not from describing it: *"it feels so balanced"* → bounces it on her
palm at arm's length; *"listen to that clasp"* → thumb clicks it open. Find the one
or two moments that are genuinely satisfying to watch in motion and build beats
around them.

**Detail-in-motion, not description.** You may name a part to tell the model what to
feature (*"presses the clasp,"* *"pans down the shaft to the grip"*) — that's
direction, not description. What you avoid is stating what the product *is* (its
color, material, branding) — the image already says that.

### Step 5.5 — Bind the references explicitly

The prompt must state **which attached asset governs what**, and declare the assets
the source of truth. Without this the model treats references as loose inspiration
and drifts — a slightly different face by the last beat, an outfit that changes
fabric mid-clip. Naming each asset and tying it to a specific job holds identity
across all 15 seconds.

The binding is written the same way in both modes, because Seedance 2.0 reads
`@Image1` / `@Image2` / `@Image3` natively: each label points at the image in that
position of the `image_urls` list (API mode) or the attach order (Manual mode).
Number the labels in the exact order the images are passed, and open the prompt
with a binding line:

> The appearance of the model and the product comes entirely from the attached
> assets. @Image1 locks her face, identity, outfit, and body — match it exactly,
> same clothing, same fabric, same fit. @Image2 is the product, and it stays exactly
> as @Image2 shows it. @Image3 shows the true size of the product in her hand — keep
> that scale. Never describe their appearance in words — the assets are the source
> of truth.

The standard attach order for this pipeline:

| Label | Asset | Governs |
|---|---|---|
| `@Image1` | Character reference sheet (Step 2) | face, identity, hair, outfit, body |
| `@Image2` | Product reference sheet (Step 3) | product look — shape, label, color, material |
| `@Image3` | Product scale reference (Step 4) | product size relative to her hands and body |

If the user skipped Step 4, pass only two images and drop the `@Image3` sentence from
the binding line.

Then refer to the product as `the @Image2 [item]` at every beat, not just the first.
In Manual mode always tell the user the attach order explicitly, since the numbering
is positional and silently breaks if they reorder. In API mode the order of
`image_urls` is the order — never shuffle it after writing the prompt.

Never show the user a raw request body, a public upload URL, or a machine model id
unless they ask. Refer to assets by their friendly names ("your character sheet",
"the product sheet").

### Step 6 — Match dialogue density to the 15-second runtime

Every video here is **15 seconds** — one clip, no exceptions. That window holds
**three to four short lines**, and no more. Overwriting is the most common failure:
crammed dialogue makes the model rush the delivery and flattens the hero moment.

Fifteen seconds fits the full natural arc: **hook → reaction → specific detail →
opinion → casual sign-off** (e.g. *"That's it. That's the review."*), with the
detail and opinion often sharing a line. Dialogue is casual, genuine, and a little
imperfect — like talking to a friend, not reading a script.

### Step 6.5 — Confirm the accent before generating

The speaking accent strongly shapes how the video reads, and the user often has a
specific market in mind. Before you generate the prompt, **ask the user which accent
they want**, telling them the default. Phrase it simply, e.g.:

> "By default I'll set the delivery to a neutral American accent — would you prefer a
> different one (British, Australian, etc.)?"

Default to a **neutral American accent** if they don't care or don't answer. Once
chosen, write the accent into the creator's delivery description so the model voices
it — e.g. *"…soft-spoken with a neutral American accent, like she's showing a close
friend."* Ask this once per video; don't re-ask on minor revisions.

### Step 7 — Close with the realism block (always)

End every prompt with a closing block that does three jobs, in this order. Diffusion
models steer better toward a described target than away from a forbidden one, so
**lead with what the footage should be, then forbid**.

**a) Positive realism statement.** State the target directly:

> The video looks and sounds like real iPhone footage — authentic UGC aesthetic,
> iPhone HDR, natural skin texture, slight handheld shake.

Drop "slight handheld shake" when the rig is mounted or propped — a locked-off phone
does not shake.

**b) Sound direction.** Seedance generates native audio, so silence on this point is
a wasted control. Always specify the voice and the room:

> Natural unprocessed voice and quiet [room] tone fitting the [setting].

Match the room tone to the scene actually described (a warm kitchen, a quiet bedroom,
an outdoor court). Never request music — see the negative line.

**c) Negative constraints.** Then suppress the remaining AI tells:

> No music, no filters, no text overlays, no overly polished ad look, no fake studio
> lighting, no unrealistic skin, no robotic delivery. The phone, the camera, and the
> hands holding the phone or camera are never visible.

The **crew-invisibility clause is mandatory and easy to forget.** A selfie-rig prompt
describes a phone that the finished shot must never contain; without this line the
model frequently renders a second phone, a visible camera, or a disembodied hand
holding the rig. Include it every time, on every rig.

Add **"no camera zoom, locked frame"** whenever the rig is mounted/propped.

## Assembly template

Assemble the parts into flowing prose (not labels). Structure:

```
[technical header — one line] [asset-binding line] [scene + mood] [creator energy +
camera relationship; wardrobe only if overriding].

(0–4s) [Label]: [action + camera move]: "[line]"
(4–8s) [Label]: [action + camera move]: "[line]"
(8–12s) [Label]: [action + camera move]: "[line]"
(12–15s) [Label]: [action + camera move]: "[final line]"

[positive realism statement] [sound direction] [negative constraints + crew-invisibility]
```

For jump cuts, replace the timestamped beats with labeled segments
(`HOOK (0–2s)`, `JUMP CUT 1 (2–4s)`, … `OUTRO`), each naming shot type + angle +
action + a sound/motion cue.

## Worked example

**Input:** product image of a vintage beaded evening clutch; avatar image of a
woman in a black silk cami + relaxed trousers. Request: keep the reference outfit.
(Runtime is always 15 seconds — never asked, never varied.)

**Reasoning:** Detail-rich product whose magic is the beadwork catching light and
the clasp clicking — tactile, so continuous take, not montage. The clasp wants two
hands → prop the phone, locked frame, no zoom. Outfit kept → no wardrobe override.
15 seconds → four short lines across four beats, with room to let the clasp moment
breathe.

**Output:**

> Vertical 9:16 selfie-style UGC video, shot on iPhone front camera. The phone is
> propped up on a vanity table, leaning against a stack of books, framing the woman
> from the chest up as she sits in front of it — a fixed, locked-off shot, no
> handheld movement, no zoom. Warm natural indoor light, casual framing, real skin
> tones, no filters, intimate low-key cozy energy.
>
> The appearance of the model and the product comes entirely from the attached
> assets. @Image1 locks her face, identity, outfit, and body — match it exactly,
> same clothing, same fabric, same fit. @Image2 is the beaded clutch, and it stays
> exactly as @Image2 shows it. @Image3 shows the true size of the clutch in her
> hand — keep that scale. Never describe their appearance in words — the assets are
> the source of truth.
>
> A softly lit bedroom with warm daylight from the side, calm and intimate mood. A
> young woman, calm and a little awe-struck, soft-spoken with a neutral American
> accent like she's showing a close friend, looking directly into the lens.
>
> (0–4s) Opener: She holds the @Image2 clutch up toward the propped phone with both
> hands, slowly tilting it so it catches the warm light: "Okay. I have to show you
> this one." (4–8s) Use: She turns the @Image2 clutch slowly through the light,
> leaning in toward the lens: "Look at the beadwork when it moves." (8–12s) Result:
> Her thumb presses the clasp so it clicks open, then shut again: "And listen to
> that clasp — that's the whole thing." (12–15s) Verdict: She sits back and holds
> the @Image2 clutch beside her face, a soft smile into the lens: "It's so
> beautiful. That's the review."
>
> The video looks and sounds like real iPhone footage — authentic UGC aesthetic,
> iPhone HDR, natural skin texture. Natural unprocessed voice and quiet room tone
> fitting the calm bedroom. No music, no filters, no text overlays, no camera zoom,
> locked frame, no overly polished ad look, no robotic delivery. The phone, the
> camera, and the hands holding the phone or camera are never visible.

**Attach order (both modes):** Image1 = character reference sheet, Image2 = product
reference sheet, Image3 = product scale reference. In API mode this is the order of
`image_urls`; in Manual mode it is the order the user attaches them.

## Quick checklist before delivering

- Is the prompt fully deterministic — zero "or"s, no unresolved options? Any real
  choice was asked of the user, not hedged into the text?
- Accent confirmed with the user (default neutral American) and written into the
  creator's delivery?
- Technical header present? Closing realism block present — positive statement, then
  sound direction, then negatives?
- **Crew-invisibility clause present?** (phone / camera / holding hands never visible)
- Sound direction names a room tone matching the actual scene?
- Assets bound explicitly — each one tied to a specific job, declared source of truth,
  and the product re-referenced by its asset label at every beat (not just the first)?
- `@ImageN` labels match the `image_urls` order (API mode) or the attach order
  (Manual mode) exactly — Image1 character sheet, Image2 product sheet, Image3 scale
  reference (omitted if Step 4 was skipped)?
- Attach order stated to the user (Manual mode)?
- Every beat carries a function label, and no two labels repeat?
- Every beat timestamped?
- Any appearance description of product or model that the image already covers? Cut it.
- Does the rig physically support every action? (No two-handed action on a handheld
  shot.) Mounted → frame locked, no zoom?
- Wardrobe described only if the user is overriding the reference?
- Exactly one clip, beats spanning the full 15 seconds, with three to four short
  lines and no more?
