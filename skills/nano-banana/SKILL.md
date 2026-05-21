---
name: nano-banana
description: This skill should be used when the user wants to generate images, edit photos, design YouTube thumbnails, create infographics, build storyboards, or translate text inside images using Nano Banana (Google's Gemini image generation models). Applies to any request involving Nano Banana, Nano Banana 2, Nano Banana Pro, Gemini image generation, Gemini 2.5 Flash Image, Gemini 3 Pro Image, Gemini 3.1 Flash Image, AI photo editing, conversational image editing, multi-character consistency (up to 5 characters), multi-object reference compositions (up to 14 reference images), in-image text translation, image search grounding, or photo restoration. Also covers casual requests such as "help me make an image", "generate a thumbnail", or "can Gemini edit this photo".
---

# Nano Banana Prompting Guide (2026)

## Model Lineup

Nano Banana is Google's marketing name for the Gemini image generation models. Three models are currently exposed via the Gemini API:

| Marketing name | Model code | Status | Updated | Best for |
|---|---|---|---|---|
| **Nano Banana** | `gemini-2.5-flash-image` | **Stable** | Oct 2025 | High-volume generation, conversational editing, low-latency creative workflows |
| **Nano Banana 2** | `gemini-3.1-flash-image-preview` | Preview | Feb 2026 | Mainstream-price upgrade with thinking, search grounding, 0.5K-4K output, 14 aspect ratios |
| **Nano Banana Pro** | `gemini-3-pro-image-preview` | Preview | Nov 2025 | Studio-quality precision, complex graphic design, factual data visualizations, accurate text |

**Defaults:**
- For most production use: **Nano Banana** (`gemini-2.5-flash-image`) - the only stable model.
- For text rendering, infographics, search-grounded factual imagery, or low-resolution thumbnails (0.5K): **Nano Banana 2** (Preview - features may change before going stable).
- For high-stakes design work where text accuracy, complex composition, and reasoning matter: **Nano Banana Pro** (Preview, most expensive).

Preview models may change before going stable and have more restrictive rate limits ([source](https://ai.google.dev/gemini-api/docs/pricing)).

---

## Quick Start: Generate or Edit an Image in 60 Seconds

**Generate:** Describe your scene in a full sentence with style and specs:
```
A professional product photo of a black leather wallet on white marble.
Soft studio lighting, macro lens, shallow depth of field. 2K, 3:2 aspect ratio.
```

**Edit:** Stay in the same conversation and describe only what changes:
```
Turn 1: "A red convertible parked on a coastal road at sunset."
Turn 2: "Change the color to midnight blue."
Turn 3: "Add surfboards on the roof rack."
```

The model carries the full visual context through the conversation - you describe only what changes, not the entire scene again.

For prompt formulas, model-specific guidance, and advanced techniques - read on.

---

## Language of Prompts

Always write Nano Banana prompts in **English**, regardless of the language the user is writing in. The model was trained predominantly on English and produces noticeably better results with English prompts.

Workflow:
- Talk to the user in their language (Polish, German, French, etc.).
- Write the final prompt in English.
- If the user describes what they want in another language, translate to English before presenting the prompt.

Exception: text that should appear **inside the image** (e.g., a Polish poster headline, a Japanese label) stays in the target language. The surrounding prompt instructions stay in English.

Nano Banana 2 specifically advertises "improved i18n text rendering" ([source](https://ai.google.dev/gemini-api/docs/models/gemini-3.1-flash-image-preview)), so non-Latin scripts and accented Latin characters render more reliably than on the original Nano Banana - but the prompt itself still works best in English.

---

## Golden Rules

1. **Creative Director, not keyword vomit** - Write full sentences describing a scene, not tag lists.
2. **Edit, don't re-roll** - If an image is 80% right, ask for specific changes. Preserve the 80% that works.
3. **Explain the logic** - Nano Banana 2 and Pro are thinking models ([Pro spec](https://ai.google.dev/gemini-api/docs/models/gemini-3-pro-image-preview)). They reason through intent. Say *why*, not just *what*.
4. **Specify preservation explicitly** - Always state what must not change during edits.
5. **Provide context and purpose** - "For a Brazilian gourmet cookbook" helps the model make better artistic decisions.
6. **Use positive framing** - Describe what you want, not what you don't want.

---

## Core Prompt Structure (6 Factors)

Every strong prompt covers these six factors:

```
[SUBJECT] - who or what is in the image
[ACTION] - what is happening
[SETTING/LOCATION] - where the scene takes place
[COMPOSITION] - camera angle, framing, shot type
[LIGHTING/ATMOSPHERE] - quality and mood of light
[STYLE/MEDIUM] - art style, look and feel
```

### Text-to-Image Template

```
[Subject + adjectives] doing [action] in [setting].
[Composition/camera angle]. [Lighting/atmosphere].
[Style/medium]. [Technical specs: resolution, aspect ratio].
[Constraints or text to render if any]
```

**Example:**
```
A stoic robot barista with glowing blue optics preparing an espresso
in a minimalist Scandinavian cafe. Wide shot, rule of thirds.
Soft morning light through large windows. Cinematic 3D, photorealistic.
2K, 16:9, shallow depth of field.
```

### Image Editing Template

```
[Action verb] + [specific element] + [desired result].
KEEP UNCHANGED: [list of what must stay the same].
[Style/quality requirements].
```

**Action verbs:** Replace, Remove, Add, Change, Transform, Colorize, Translate, Enhance, Restore

**Example:**
```
Replace the white top with a black t-shirt.
KEEP UNCHANGED: face, hairstyle, pose, background, lighting.
Photorealistic, natural shadows. Seamless integration.
```

---

## Reference Images (Multi-Image Composition)

The Gemini 3 image models accept up to **14 reference images** in a single generation ([source](https://ai.google.dev/gemini-api/docs/image-generation#use_up_to_14_reference_images)). Capacity per model:

| Capacity | Nano Banana 2 (`gemini-3.1-flash-image-preview`) | Nano Banana Pro (`gemini-3-pro-image-preview`) |
|---|---|---|
| Objects with high-fidelity | up to 10 | up to 6 |
| Characters with consistency | up to 4 | up to 5 |
| Total reference images | 14 | 14 |

The original **Nano Banana** (`gemini-2.5-flash-image`) works best with up to **3 input images** ([source](https://ai.google.dev/gemini-api/docs/image-generation), Tips & Tricks section).

### Role-assignment pattern (works for any model):

```
Image 1 [CHARACTER/IDENTITY]: Keep this person's face exactly
Image 2 [POSE/COMPOSITION]: Use this framing and body position
Image 3 [STYLE/AESTHETIC]: Match this color palette and mood
Image 4 [LIGHTING]: Replicate this lighting setup
Image 5 [ENVIRONMENT/BACKGROUND]: Use this setting
```

**Example multi-reference prompt:**
```
Combine the subject from Image 1 (keep face identical) with the setting
from Image 2. Apply the lighting style from Image 3.
Seamlessly blend all elements, match perspective and shadows.
Photorealistic. 2K, 3:2 aspect ratio.
```

---

## Multi-Character and Object Consistency

To maintain visual consistency across multiple characters in a single image (group photo, comic panel, storyboard):

```
Scene features [CHARACTER A] (tall woman, red coat, short dark hair)
and [CHARACTER B] (older man, grey beard, blue jacket).
Keep both characters visually identical to the reference images.
[Scene description, action, setting, composition]
```

### Sequential consistency (storyboarding):

Use conversational editing - stay in the same chat and reference earlier images:
```
Continue this story from the previous image. The main character -
same as Image 1 - is now doing [new action] in [new location].
KEEP IDENTICAL: character appearance, clothing, hair, distinguishing features.
CHANGE: [what is different in this scene].
```

### 360-degree character view:

Documented in the official guide ([source](https://ai.google.dev/gemini-api/docs/image-generation#character_consistency_360_view)): iteratively prompt for additional angles in the same chat, referencing the first image to maintain consistency. For complex poses, include a reference image of the desired pose.

---

## Text Rendering and In-Image Translation

### Text rendering:

Specify text in quotes, with font style and placement:
```
Create a poster. Main headline: "AUTUMN SPECIAL" in bold gold serif at top.
Subtitle: "Limited Time Only" in clean modern badge style, right side.
Footer: "Offer ends Oct 31" in small clean text at bottom.
Ensure all text is perfectly spelled, ultra-sharp, high contrast.
```

Pro and Nano Banana 2 produce noticeably sharper, more reliable text than the original Nano Banana. For dense layouts (menus, infographics, multi-language posters), prefer **Pro**.

### In-image translation:

The official docs demonstrate translating an existing image's text into another language while preserving the design ([source](https://ai.google.dev/gemini-api/docs/image-generation#text_rendering), Spanish infographic example):
```
Update this image to [TARGET LANGUAGE]. Do not change any other
elements of the image. Match the original font style and weight.
All translated text must be perfectly legible.
```

The docs do not enumerate a supported language list - verify against the current docs if a specific script matters.

---

## Conversational Editing

Iterate naturally in the same chat. The model holds the full visual context, so describe only what changes:

```
Turn 1: "A corporate headshot of a woman in a navy suit, white background, studio lighting."
Turn 2: "Change the background to a blurred modern office."
Turn 3: "Add glasses."
Turn 4: "Make the lighting warmer, golden hour feel."
```

Use this instead of starting from scratch when one element needs adjusting.

---

## Image Search Grounding (Nano Banana 2 and Pro)

Both Nano Banana 2 and Pro can ground generations on Google Search results - including image search - for factually accurate visuals ([source](https://ai.google.dev/gemini-api/docs/image-generation), Grounding section). Original Nano Banana does **not** support search grounding.

Use cases:
- Weather charts with current data
- Diagrams of real-world places (floor plans, maps, building layouts)
- Reference visuals for products, vehicles, or landmarks the model may not have seen

```
Visualize the current weather forecast for Tokyo for the next 5 days
as a modern weather chart with temperature values, condition icons,
and day labels. Clean professional style, 16:9, 1920x1080px.
```

Grounding is billed per search query on top of the image price (5,000/month free across the Gemini 3 family, then $14 / 1,000 queries) - see `references/pricing.md`.

---

## Storyboards for Video Workflows

Nano Banana is documented as the starting point for storyboard frames that can be handed to Veo (Google's video model) ([source](https://ai.google.dev/gemini-api/docs/image-generation), "Bonus: Comic strips and storyboards" + link to Veo guide at the end of the page).

**Storyboard workflow:**
1. Generate the first key frame in Nano Banana with full character/scene specification.
2. Use conversational editing to produce subsequent frames - reference the previous frame for consistency.
3. Export the frames and use them as image inputs to Veo for motion generation.

**Frame prompt template:**
```
Storyboard frame [N of total]. [Subject description with all consistency details]
doing [specific action] in [setting].
[Precise camera angle and framing - e.g. "low angle, wide shot"].
[Lighting suitable for video, e.g. "soft even diffuse light"].
Style: [art direction consistent with other frames].
Clear foreground/background separation.
```

Veo motion-from-keyframes specifics are out of scope for this skill - see the official [Veo guide](https://ai.google.dev/gemini-api/docs/video) for the handoff details.

---

## YouTube Thumbnails

### Structure:
```
[Subject positioning] + [Expression/action] + [Text overlay specs]
+ [Background treatment] + [Contrast/color requirements] + [Technical specs]
```

### Key principles:
- **High contrast** - mobile-readable, minimum 30% contrast between elements.
- **Bold text** - 3-5 words max, large bold sans-serif with outline or shadow. Use Pro or Nano Banana 2 for text accuracy.
- **Strong emotion** - excited/surprised expressions tend to increase CTR.
- **Rule of thirds** - face on one side, product/text on the other.
- **Avoid bottom-right** - YouTube timestamp overlay covers this area.

### Example:
```
YouTube thumbnail for a tech review video.

SUBJECT: Person with excited expression, eyes wide, positioned left 2/3 of frame
PRODUCT: [PRODUCT NAME] on right side, subtle glow effect, ~30% of frame
TEXT: "[HEADLINE 3 WORDS]" in bold sans-serif, white with black outline, top center
BACKGROUND: Blurred tech-themed, high contrast with subject
COMPOSITION: Rule of thirds, visual flow from face to product to text

TECHNICAL: 1280x720px, 16:9, high saturation, mobile-optimized
```

Set output to **1K** (or 0.5K on Nano Banana 2) for thumbnails - 2K/4K is overkill for YouTube's 1280x720 spec and costs more.

---

## Infographics

Pro is best for infographics because of text accuracy and Search Grounding for factual data. Nano Banana 2 also works.

### Best types:
- Timeline infographics
- Comparison charts (side-by-side)
- Process / How-to diagrams
- Statistical KPI cards
- Educational concept diagrams

### Framework:
```
[Type] infographic: "[TITLE]"

STRUCTURE:
- [Section 1 description]
- [Section 2 description]
- [Section 3 description]

DESIGN:
- Colors: Primary [HEX], Secondary [HEX], Text [HEX], Background [HEX]
- Typography: [font style], headlines [size]pt, body [size]pt
- Layout: generous whitespace, [padding]px padding

TECHNICAL: [resolution], [aspect ratio], high contrast, all text ultra-sharp

STYLE NOTE: Flat design, clean lines. Avoid drop shadows, 3D effects, and gradients on charts.
```

For data-grounded infographics (e.g., "show GDP growth of top-5 economies last quarter"), enable Search Grounding by using Nano Banana 2 or Pro and including a phrase like "use real, current data".

---

## Technical Specs

### Resolutions (output)

| Model | 0.5K (512px) | 1K | 2K | 4K |
|---|---|---|---|---|
| `gemini-2.5-flash-image` | - | ✅ (up to 1024x1024) | - | - |
| `gemini-3.1-flash-image-preview` | ✅ | ✅ | ✅ | ✅ |
| `gemini-3-pro-image-preview` | - | ✅ | ✅ | ✅ |

Sources: [3.1 Flash spec](https://ai.google.dev/gemini-api/docs/models/gemini-3.1-flash-image-preview), [pricing footnotes](https://ai.google.dev/gemini-api/docs/pricing).

### Aspect ratios (Nano Banana 2 and Pro)

`1:1`, `1:4`, `1:8`, `2:3`, `3:2`, `3:4`, `4:1`, `4:3`, `4:5`, `5:4`, `8:1`, `9:16`, `16:9`, `21:9`

Nano Banana 2 added the extreme ratios `1:4`, `4:1`, `1:8`, `8:1` ([source](https://ai.google.dev/gemini-api/docs/models/gemini-3.1-flash-image-preview)).

### Watermarking

All generated images carry a **SynthID** watermark ([source](https://ai.google.dev/responsible/docs/safeguards/synthid)). The official Gemini API image-generation docs do not mention C2PA Content Credentials - if you need C2PA for a compliance reason, verify against the latest docs before promising it.

### Capabilities matrix

| Capability | Nano Banana (2.5) | Nano Banana 2 (3.1) | Nano Banana Pro (3 Pro) |
|---|---|---|---|
| Image generation | ✅ | ✅ | ✅ |
| Conversational editing | ✅ | ✅ | ✅ |
| Thinking | ❌ | ✅ | ✅ |
| Search grounding (web) | ❌ | ✅ | ✅ |
| Search grounding (image) | ❌ | ✅ | ✅ |
| Batch API | ✅ | ✅ | ✅ |
| Context caching | ✅ | ❌ | ❌ |
| Structured outputs | ✅ | ❌ | ✅ |

Sources: per-model spec pages linked at end.

### Access points

- **Gemini API** (`generativelanguage.googleapis.com`) - direct programmatic access
- **Google AI Studio** (aistudio.google.com) - free interactive playground for all three models
- **Vertex AI** - enterprise deployment

Consumer surfaces (Gemini app, Google AI Studio) are out of scope for API prompt-writing but use the same underlying models.

---

## Pricing (snapshot)

Detailed pricing in `references/pricing.md`. Headline numbers ([source](https://ai.google.dev/gemini-api/docs/pricing), verified 2026-05-21):

| Model | Per-image output (Standard, paid tier) |
|---|---|
| `gemini-2.5-flash-image` | $0.039 (up to 1024x1024) |
| `gemini-3.1-flash-image-preview` | $0.045 (0.5K), $0.067 (1K), $0.101 (2K), $0.151 (4K) |
| `gemini-3-pro-image-preview` | $0.134 (1K-2K), $0.24 (4K) |

**No free tier for image generation models.** Free Tier shows "Not available" for all three. The free Google AI Studio playground exists but counts against per-account quotas, not a billing-free image quota.

**Batch API** halves the per-image cost on all three models.

---

## Common Issues and Fixes

| Problem | Solution |
|---|---|
| Text blurry or misspelled | Switch to Nano Banana 2 or Pro. Add: "All text ultra-sharp, perfectly spelled, high resolution" |
| Face changes during edit | Add: "KEEP UNCHANGED: face must be 100% identical to original" |
| Colors don't match brand | Use HEX codes: "background color: #0066CC" |
| Composition shifts | Add: "Preserve exact composition, framing, and layout" |
| Inconsistent style | Specify one dominant style: "Professional photography ONLY. No filters." |
| Character drifts across images | Supply reference image + "Character appearance is LOCKED to Image 1" |
| Translation looks wrong | Add: "Match original font weight, match text box size, do not rearrange layout" |
| Need accurate factual visual | Use Nano Banana 2 or Pro with "use real, current data" - triggers Search Grounding |
| 4K image too expensive | Generate at 2K, upscale offline; or use Batch API for 50% discount |
| Need PNG output | Save the returned bytes locally with `.png` extension - models return image bytes, not URLs |

---

## What NOT to Do

- **Tag soup**: `dog, park, 4k, realistic, sunny, professional`
- **Vague descriptions**: "nice image", "good quality", "make it better"
- **Cliche boosts**: "trending on artstation, masterpiece, 8k ultra" - the model ignores these
- **Re-rolling from scratch** when 80% is already right - edit conversationally instead
- **Missing preservation instructions** during edits
- **Mixing conflicting styles** in one prompt
- **Over-prompting** - 500+ word prompts confuse rather than clarify
- **Celebrity names** - use descriptive attributes instead ("woman with sharp cheekbones and red hair")
- **Picking Pro for thumbnails** - Pro is for high-fidelity work; thumbnails don't need it and pay 3-4x more per image
- **Treating Preview models as stable** - Nano Banana 2 and Pro are Preview and may change without notice

---

## Quick Reference Card

**Always include:**
- Subject + action + setting
- Lighting conditions
- Art style / medium
- Technical specs (resolution, aspect ratio)
- What to preserve (for edits)

**Model picks:**
- High-volume / conversational editing / lowest cost → **Nano Banana** (`gemini-2.5-flash-image`)
- Thumbnails with text / 0.5K-4K output / current-data infographics → **Nano Banana 2** (`gemini-3.1-flash-image-preview`)
- Studio-quality design / dense text / professional infographics → **Nano Banana Pro** (`gemini-3-pro-image-preview`)

For ready-to-use prompts by category, see `references/prompt-examples.md`. For pricing breakdown by tier (Standard, Batch, Flex, Priority), see `references/pricing.md`.

---

## Verification & Sources

Every numeric claim, model ID, capability flag, and price in this skill is sourced from Google's official Gemini API docs at `ai.google.dev/gemini-api/docs`, verified on **2026-05-21**.

- **Image generation guide**: https://ai.google.dev/gemini-api/docs/image-generation
- **Nano Banana spec** (`gemini-2.5-flash-image`): https://ai.google.dev/gemini-api/docs/models/gemini-2.5-flash-image
- **Nano Banana 2 spec** (`gemini-3.1-flash-image-preview`): https://ai.google.dev/gemini-api/docs/models/gemini-3.1-flash-image-preview
- **Nano Banana Pro spec** (`gemini-3-pro-image-preview`): https://ai.google.dev/gemini-api/docs/models/gemini-3-pro-image-preview
- **Pricing page**: https://ai.google.dev/gemini-api/docs/pricing
- **SynthID watermark**: https://ai.google.dev/responsible/docs/safeguards/synthid
- **Veo handoff** (storyboards): https://ai.google.dev/gemini-api/docs/video

Things this skill **does not** claim because the official docs do not confirm them:
- A specific "100+ languages" count for text rendering or translation - docs do not enumerate.
- C2PA Content Credentials - only SynthID is mentioned in image-generation docs.
- A formal "Veo + Lyria + Flow 4-stage pipeline" - docs only link to the Veo guide; the rest is third-party narrative.
- Default model claims for the consumer Gemini app - this skill targets the API.

If you discover a claim above is wrong, the fix is to update both this section and the relevant body section with the corrected fact and its source URL.
