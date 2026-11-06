---
name: nano-banana
description: Write and improve prompts for Nano Banana - Google's Gemini image generation models - and pick the right model for the job. Covers text-to-image, conversational photo editing, YouTube thumbnails, infographics, storyboards, video-to-image, multi-character consistency, up to 14 reference images, in-image text translation, wide banner ratios up to 8:1, and search-grounded factual imagery. Also covers which model to use (Nano Banana 2.1, Nano Banana 2 Lite, Nano Banana Pro, deprecated Nano Banana 2 and legacy Nano Banana), per-image cost, resolutions and aspect ratios.
when_to_use: Use for any request involving Nano Banana, Nano Banana 2.1, Nano Banana 2, Nano Banana 2 Lite, Nano Banana Pro, Gemini image generation, or the model codes gemini-nano-banana-2.1, gemini-3.1-flash-image, gemini-3.1-flash-lite-image, gemini-3-pro-image, gemini-2.5-flash-image. Trigger phrases include "generate an image", "make me a thumbnail", "edit this photo", "AI photo editing", "image prompt", "infographic", "translate text in this image", "restore this photo", "character consistency", "can Gemini edit this photo", "which Nano Banana model", "migrate from gemini-3.1-flash-image".
---

# Nano Banana Prompting Guide

Facts in this skill are verified against Google's official Gemini API docs, last checked **2026-11-06**. See "Verification & Sources" at the bottom. Model IDs and prices change - re-verify before quoting them in a deliverable.

## Model Lineup

Nano Banana is Google's marketing name for Gemini's native image generation. **Five** models are exposed via the Gemini API, all of them Stable, two of them deprecated:

| Marketing name | Model code | Status | Updated | Best for |
|---|---|---|---|---|
| **Nano Banana 2.1** | `gemini-nano-banana-2.1` | Stable (GA 2026-10-06) | Oct 2026 | **Default workhorse, Google's recommendation for all new projects.** 1K-4K, web + image search grounding, 14 aspect ratios, up to 14 reference images, video and PDF input |
| **Nano Banana 2 Lite** | `gemini-3.1-flash-lite-image` | Stable (GA 2026-06-30) | Jun 2026 | Latency: sub-2s, lowest input and thinking cost, fast local edits. 1K only |
| **Nano Banana Pro** | `gemini-3-pro-image` | Stable (GA 2026-05-28) | Nov 2025 | Studio-quality precision, dense/accurate text, complex graphic design, factual data visualizations, brand consistency |
| **Nano Banana 2** | `gemini-3.1-flash-image` | ⚠️ **Deprecated 2026-10-06, no shutdown date announced** | Feb 2026 | Existing code only. Migrate to Nano Banana 2.1 |
| **Nano Banana** (legacy) | `gemini-2.5-flash-image` | ⚠️ **Deprecated - shutdown 2027-03-15** | Oct 2025 | Nothing new. Migrate to Nano Banana 2 Lite |

Note the naming: `gemini-nano-banana-2.1` drops the `gemini-<version>-flash-image` pattern. It is an update to Nano Banana 2, not a different product line, so do not guess its ID from the old scheme.

**Defaults - pick in this order:**
1. **Nano Banana 2.1** (`gemini-nano-banana-2.1`) - the right default for almost everything: thumbnails, product shots, multi-reference composition, infographics, wide banners, 4K finals. Google describes it as keeping Flash-level speed and cost while improving visual quality, prompt adherence, multi-turn character consistency and text rendering ([card](https://aistudio.google.com/docs/models/gemini-nano-banana-2.1)).
2. **Nano Banana 2 Lite** (`gemini-3.1-flash-lite-image`) - when latency is the constraint: a snappy interactive edit loop (colour swaps, stickers, background changes) or real-time use. It no longer undercuts the default on image output: at 1K both cost $0.0336. Lite is cheaper on input and on text/thinking tokens only.
3. **Nano Banana Pro** (`gemini-3-pro-image`) - when text must be flawless, layout is complex, or brand/factual accuracy is the point. About 4x Nano Banana 2.1 per image at 1K, about 2x at 4K.

### ⚠️ Migration warning (state as of 2026-11-06)

- **`gemini-3.1-flash-image` is deprecated** since 2026-10-06, no shutdown date announced ([changelog](https://aistudio.google.com/docs/changelog), [deprecations](https://aistudio.google.com/docs/deprecations)). Move to `gemini-nano-banana-2.1`. What changes when you swap the ID: **0.5K output is gone**, default Thinking goes from `minimal` to `medium`, image output gets cheaper at every resolution, but input ($0.50 → $1.50 per 1M tokens) and text/thinking output ($3 → $7.50 per 1M) get dearer. Reference-image limits and aspect ratios stay the same.
- **`gemini-2.5-flash-image` is deprecated**, shutdown **2027-03-15**, later than the 2026-10-02 date published earlier. Google's recommended replacement is **Nano Banana 2 Lite** (`gemini-3.1-flash-lite-image`). If a user's code or notes reference it, flag this.
- **Treat the `-preview` IDs as dead.** The deprecations page lists `gemini-3.1-flash-image-preview` and `gemini-3-pro-image-preview` under deprecated models, shutdown **2026-06-25**, with replacements `gemini-nano-banana-2.1` and `gemini-3-pro-image`. The API's model list still returned both on 2026-11-06, along with an undocumented `nano-banana-pro-preview`, so a listing proves nothing either way. Never emit a `-preview` image model ID.
- **Imagen is shut down** - `imagen-4.0-*` went on **2026-08-17**. Use Nano Banana models for all image generation.
- Shutdown dates on the deprecations page are the *earliest possible* dates; Google says it will give advance notice of the exact date. Re-check before planning around one.

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

**Official best-performance language list** ([source](https://aistudio.google.com/docs/image-generation#limitations)): `EN`, `ar-EG`, `de-DE`, `es-MX`, `fr-FR`, `hi-IN`, `id-ID`, `it-IT`, `ja-JP`, `ko-KR`, `pt-BR`, `ru-RU`, `ua-UA`, `vi-VN`, `zh-CN`.

**Polish is not on that list.** Practical consequence: Polish prompt text is a bad idea (write English), and Polish text *rendered inside* an image is not a guaranteed-quality path. It usually works - Nano Banana 2 advertised "improved i18n text rendering" ([card](https://aistudio.google.com/docs/models/gemini-3.1-flash-image)), 2.1 adds further text-rendering improvements, and Polish diacritics are Latin - but for a deliverable, treat it as needing a proof render. If the diacritics come out wrong (ą, ę, ś, ż, ź, ć, ń, ł), the reliable fallback is: generate the image without text, then set the headline as a real text layer in post. The same applies to any language outside the list.

**Two-step trick for text-heavy images** (straight from the official docs): *"When generating text for an image, Gemini works best if you first generate the text and then ask for an image with the text."* So write the exact copy in a first turn, then ask for the image in a second turn - don't make the model invent the copy and lay it out in one shot.

---

## Golden Rules

1. **Creative Director, not keyword vomit** - Write full sentences describing a scene, not tag lists.
2. **Edit, don't re-roll** - If an image is 80% right, ask for specific changes. Preserve the 80% that works.
3. **Explain the logic** - the Gemini 3 image models are thinking models: Thinking is on by default and cannot be disabled in the API ([source](https://aistudio.google.com/docs/image-generation#thinking-process)). They reason through intent. Say *why*, not just *what*.
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

The Gemini 3 image models accept up to **14 reference images** in a single generation ([source](https://aistudio.google.com/docs/image-generation#use-14-images)). Capacity per model, per the official table:

| Capacity | Nano Banana 2.1 and 2 | Nano Banana Pro (`gemini-3-pro-image`) |
|---|---|---|
| Objects with high fidelity | up to 10 | up to 6 |
| Characters with consistency | up to 4 | up to 5 |
| Style references | - | up to 3 |
| Total reference images | 14 | 14 |

**Nano Banana 2 Lite is not the model for this.** The guide states it is "not optimized for multiple reference inputs or multi-turn sequential editing" - if the job needs several references or a long edit chain, use Nano Banana 2.1.

Legacy **Nano Banana** (`gemini-2.5-flash-image`) works best with up to **3 input images** ([limitations](https://aistudio.google.com/docs/image-generation#limitations)).

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

Documented in the official guide ([source](https://aistudio.google.com/docs/image-generation#7-character-consistency-360-view)): iteratively prompt for additional angles, including previously generated images in later prompts to maintain consistency. For complex poses, include a reference image of the desired pose.

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

Pro and Nano Banana 2.1 produce noticeably sharper, more reliable text than the original Nano Banana. The guide points to Pro for professional asset production, so for dense layouts (menus, infographics, multi-language posters) prefer **Pro**.

### In-image translation:

The official docs demonstrate translating an existing image's text into another language while preserving the design ([source](https://aistudio.google.com/docs/image-generation#multi-turn-image-editing), Spanish infographic example):
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

## Image Search Grounding (Nano Banana 2.1, Nano Banana 2 and Pro)

Nano Banana 2.1, Nano Banana 2 and Pro can ground generations on Google Search results for factually accurate visuals ([source](https://aistudio.google.com/docs/image-generation#use-with-grounding)). Plain web grounding does **not** pass image results to the model. **Google Image Search** grounding does, and it exists on Nano Banana 2.1 and Nano Banana 2 only: set the `google_search` tool with `search_types` including `image_search` ([source](https://aistudio.google.com/docs/image-generation#image-search)).

Not supported by **Nano Banana 2 Lite** or legacy **Nano Banana** (2.5) - if grounding matters, those two are out.

**Documented limitation:** on Nano Banana 2.1 and Nano Banana 2, Search grounding "does not support using real-world images of people from web search at this time" ([limitations](https://aistudio.google.com/docs/image-generation#limitations)). Don't promise a grounded likeness of a real person.

**Display requirement:** when you use Image Search grounding you must display the `search_suggestions` from the `google_search_result` step. That matters for anything shipped to end users.

Use cases:
- Weather charts with current data
- Diagrams of real-world places (floor plans, maps, building layouts)
- Reference visuals for products, vehicles, or landmarks the model may not have seen

```
Visualize the current weather forecast for Tokyo for the next 5 days
as a modern weather chart with temperature values, condition icons,
and day labels. Clean professional style, 16:9, 1920x1080px.
```

Grounding is billed per search query on top of the image price (5,000/month free across Gemini 3 and newer models, then $14 / 1,000 requests, default cap 1,500 requests per day) - see `references/pricing.md`.

---

## Video-to-Image (Nano Banana 2.1, Nano Banana 2, Lite)

Nano Banana 2.1, Nano Banana 2 and Nano Banana 2 Lite accept a **video** as multimodal context - either a public YouTube URL or a file uploaded via the Files API - and generate a still from it ([source](https://aistudio.google.com/docs/image-generation#video-to-image)). The model reads the frames for visual themes and key events, then combines them with your text prompt.

This is the single best fit for the YouTube workflow: thumbnail straight from the finished video, instead of describing the video from memory. Use Nano Banana 2.1 for it.

**Not supported on Pro or legacy 2.5.**

**Prompt patterns:**
```
Generate a YouTube thumbnail from this video.
Pick the single most dramatic moment as the base composition.
Add the headline "[3-4 WORDS]" in bold sans-serif, white with a heavy black outline, top-left.
Keep the presenter's face large, sharp and unobstructed. High saturation, mobile-readable contrast.
16:9, 1K.
```

```
Generate a cinematic poster that captures the key themes of this video.
No text. Leave clean negative space in the lower third for a title added later.
```

```
Generate a summary infographic of what this video covers: 4 numbered sections,
short labels only, flat design, generous whitespace. 4:5, 2K.
```

Practical notes: ask for one specific frame-moment rather than "the best moment" when you know what you want; the model does not guarantee a frame-accurate crop - it synthesizes a new image informed by the video, so treat it as art direction, not extraction. If you need the literal frame, pull it with ffmpeg and use it as a reference image instead.

---

## Storyboards for Video Workflows

Sequential art is a documented Nano Banana use case ([source](https://aistudio.google.com/docs/image-generation#6-sequential-art-comic-panel-storyboard)); the guide says it works best on Pro and the Flash-class models, which today means Nano Banana 2.1.

**Storyboard workflow:**
1. Generate the first key frame with full character/scene specification.
2. Use conversational editing to produce subsequent frames - reference the previous frame for consistency.
3. Export the frames and use them as image inputs to your video model.

**Frame prompt template:**
```
Storyboard frame [N of total]. [Subject description with all consistency details]
doing [specific action] in [setting].
[Precise camera angle and framing - e.g. "low angle, wide shot"].
[Lighting suitable for video, e.g. "soft even diffuse light"].
Style: [art direction consistent with other frames].
Clear foreground/background separation.
```

Video generation is out of scope for this skill. Veo 3.1 Preview was shut down in the Gemini API on 2026-10-22, and Gemini Omni Flash (`gemini-omni-1.2-flash`, video model, GA 2026-11-05) now takes that role there ([source](https://aistudio.google.com/docs/video)).

---

## YouTube Thumbnails

### Structure:
```
[Subject positioning] + [Expression/action] + [Text overlay specs]
+ [Background treatment] + [Contrast/color requirements] + [Technical specs]
```

### Key principles:
- **High contrast** - mobile-readable, minimum 30% contrast between elements.
- **Bold text** - 3-5 words max, large bold sans-serif with outline or shadow. Use Nano Banana 2.1 or Pro for text accuracy.
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

Set output to **1K** for thumbnails - 2K/4K is overkill for YouTube's 1280x720 spec and costs more. 1K on Nano Banana 2.1 ($0.0336) is also cheaper than 0.5K on the deprecated Nano Banana 2 ($0.045), so there is no reason to stay on the old model for drafts.

---

## Infographics

Pro is best for infographics because of text accuracy and Search Grounding for factual data. Nano Banana 2.1 is the cheaper option, with better text rendering and infographic layout accuracy than Nano Banana 2 ([card](https://aistudio.google.com/docs/models/gemini-nano-banana-2.1)).

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

For data-grounded infographics (e.g., "show GDP growth of top-5 economies last quarter"), enable Search Grounding by using Nano Banana 2.1 or Pro and including a phrase like "use real, current data".

---

## Technical Specs

### How resolution and ratio are requested

Output size is not something you write in prose - it is a parameter. In the current docs it lives in `response_format` on the Interactions API:

```json
{
  "model": "gemini-nano-banana-2.1",
  "input": "...prompt...",
  "response_format": { "type": "image", "aspect_ratio": "16:9", "image_size": "2K" }
}
```

Default behaviour: the model matches the output size to the input image, or generates a 1:1 square if there is no input image. Stating "16:9" in the prompt text helps composition but does not set the canvas - set `aspect_ratio`. `image_size` needs an uppercase K: `1k` is rejected.

Note: the sampling parameters `temperature`, `top_p` and `top_k` were **deprecated across the Gemini API on 2026-07-21** ([changelog](https://aistudio.google.com/docs/changelog)). Don't hand users prompt-tuning advice built on them.

### Resolutions (output)

| Model | 0.5K (512px) | 1K | 2K | 4K |
|---|---|---|---|---|
| `gemini-nano-banana-2.1` | - | ✅ (default) | ✅ | ✅ |
| `gemini-3.1-flash-lite-image` | - | ✅ **only** | - | - |
| `gemini-3-pro-image` | - | ✅ | ✅ | ✅ |
| `gemini-3.1-flash-image` (deprecated) | ✅ | ✅ (default) | ✅ | ✅ |
| `gemini-2.5-flash-image` (legacy) | - | ✅ (up to 1024x1024) | - | - |

0.5K exists only on the deprecated `gemini-3.1-flash-image`; the guide states it is not supported on Nano Banana 2.1.

Sources: [2.1 card](https://aistudio.google.com/docs/models/gemini-nano-banana-2.1), [3.1 Flash Lite card](https://aistudio.google.com/docs/models/gemini-3.1-flash-lite-image), [aspect-ratio tables](https://aistudio.google.com/docs/image-generation#aspect-ratios-and-image-size).

### Aspect ratios

- **Nano Banana 2.1** - 14 ratios: `1:1`, `1:4`, `1:8`, `2:3`, `3:2`, `3:4`, `4:1`, `4:3`, `4:5`, `5:4`, `8:1`, `9:16`, `16:9`, `21:9`. The extreme banner ratios (`4:1`, `8:1` and their verticals) work at 1K, 2K and 4K, and 2.1 fixes the tiling artifacts those ratios could show at 2K and 4K.
- **Nano Banana 2** (deprecated) - the same 14. If a banner at 2K or 4K shows tiling, that is the reason to migrate.
- **Nano Banana 2 Lite** - the card lists 10 ratios: `1:1`, `3:2`, `2:3`, `3:4`, `4:3`, `4:5`, `5:4`, `9:16`, `16:9`, `21:9`.
- **Nano Banana Pro** - the same 10 as Lite (no extreme banner ratios).
- **Legacy 2.5** - the same 10, single resolution.

Handy exact pixel sizes at 1K on Nano Banana 2.1: `16:9` = 1376x768, `9:16` = 768x1376, `4:5` = 928x1152, `1:1` = 1024x1024, `4:1` = 2048x512, `1:4` = 512x2048, `8:1` = 3072x384, `1:8` = 384x3072. At 2K and 4K these scale x2 and x4, so `8:1` at 4K is 12288x1536.

### Watermarking

All generated images carry a **SynthID** watermark ([source](https://ai.google.dev/responsible/docs/safeguards/synthid)). **Nano Banana 2 Lite additionally states "SynthID (Always On) + C2PA"** watermarking on its model card - it is the only model in the family where the docs name C2PA Content Credentials. For the others, don't promise C2PA.

### Capabilities matrix

| Capability | NB 2.1 | NB 2 (deprecated) | NB 2 Lite | NB Pro | Legacy 2.5 |
|---|---|---|---|---|---|
| Image generation + editing | ✅ | ✅ | ✅ | ✅ | ✅ |
| Conversational / multi-turn editing | ✅ | ✅ | ⚠️ fast local edits only | ✅ | ✅ |
| Multiple reference images | ✅ up to 14 | ✅ up to 14 | ⚠️ not optimised | ✅ up to 14 | up to 3 |
| Video input (video-to-image) | ✅ | ✅ | ✅ | ❌ | ❌ |
| PDF input | ✅ | ✅ | ✅ | ❌ | ❌ |
| Thinking levels | `minimal`, `medium` (default), `high` | `minimal` (default), `high` | `minimal` (default), `high` | on by default, levels not listed | ❌ |
| Search grounding (web) | ✅ | ✅ | ❌ | ✅ | ❌ |
| Search grounding (image) | ✅ | ✅ | ❌ | ❌ | ❌ |
| Batch API | ✅ | ✅ | ✅ | ✅ | ✅ |
| Function calling, context caching, structured outputs | ❌ | ❌ | ❌ | ❌ | ❌ |
| Flex / Priority inference | ❌ | ❌ | ❌ | ❌ | ❌ on the card, rates still on the pricing page |
| Input token limit | 131,072 | 131,072 | 65,536 | 65,536 | 65,536 |
| Output token limit | 32,768 | 32,768 | 4,096 | 32,768 | 32,768 |

Sources: per-model card pages linked at the end.

### Access points

- **Gemini API** (`generativelanguage.googleapis.com`) - direct programmatic access. The image docs are now written against the **Interactions API** (`client.interactions.create(...)`, `response_format`). A command-line client, `gemini-api`, has been in beta since 2026-10-27 ([source](https://aistudio.google.com/docs/cli)).
- **Google AI Studio** (aistudio.google.com) - free interactive playground; the right place to iterate on a prompt before spending API budget.
- **Vertex AI** - enterprise deployment.

Consumer surfaces (Gemini app) are out of scope for prompt-writing here but use the same underlying models.

---

## Pricing (snapshot)

Detailed pricing in `references/pricing.md`. Headline numbers ([source](https://aistudio.google.com/docs/pricing), verified 2026-11-06):

| Model | Per-image output (Standard, paid tier) |
|---|---|
| `gemini-nano-banana-2.1` | $0.0336 (1K), $0.0504 (2K), $0.113 (4K) |
| `gemini-3.1-flash-lite-image` | $0.0336 (1K only) |
| `gemini-3-pro-image` | $0.134 (1K and 2K), $0.24 (4K) |
| `gemini-3.1-flash-image` (deprecated) | $0.045 (0.5K), $0.067 (1K), $0.101 (2K), $0.151 (4K) |
| `gemini-2.5-flash-image` (legacy) | $0.039 (up to 1024x1024) |

Cost intuition: Nano Banana 2.1 and Lite tie at 1K on image output, and Lite wins on input ($0.25 vs $1.50 per 1M tokens) and on text/thinking ($1.50 vs $7.50 per 1M). Nano Banana 2.1 is cheaper per output image than the deprecated Nano Banana 2 at every shared resolution, half the price at 1K and 2K. Pro is about 4x Nano Banana 2.1 at 1K, about 2.7x at 2K and about 2x at 4K. Legacy 2.5 costs more per image than Lite for a worse result.

**No free tier for image generation models.** Free Tier shows "Not available" for all five. The Google AI Studio playground exists but counts against per-account quotas, not a billing-free image quota.

**Batch API** halves the cost, with published rates for Nano Banana 2.1, Lite and legacy 2.5 (up to 24h turnaround). The pricing page shows no Batch rate for Pro or the deprecated Nano Banana 2, although both model cards list Batch as supported - see `references/pricing.md`.

---

## Common Issues and Fixes

| Problem | Solution |
|---|---|
| Text blurry or misspelled | Switch to Nano Banana 2.1 or Pro (Lite is not a text model). Generate the copy in a first turn, then ask for the image. Add: "All text ultra-sharp, perfectly spelled, high resolution" |
| Polish/other off-list diacritics render wrong | Language is outside Google's best-performance list. Proof-render first; if broken, generate without text and add the headline as a real text layer in post |
| `404 model not found` on an image call | The `-preview` IDs are listed as shut down 2026-06-25. Use `gemini-nano-banana-2.1` / `gemini-3-pro-image` |
| `image_size` rejected | Lowercase `1k` is rejected, use `1K`. `0.5K` is not available on Nano Banana 2.1 |
| Tiling on a wide banner at 2K or 4K | Move from `gemini-3.1-flash-image` to `gemini-nano-banana-2.1`, which fixes it on `1:4`, `4:1`, `1:8`, `8:1` |
| Lite output looks flat vs. expectations | Lite is a speed specialist at 1K with no grounding and weak multi-reference handling. Move the job to Nano Banana 2.1, which costs the same per image at 1K |
| Face changes during edit | Add: "KEEP UNCHANGED: face must be 100% identical to original" |
| Colors don't match brand | Use HEX codes: "background color: #0066CC" |
| Composition shifts | Add: "Preserve exact composition, framing, and layout" |
| Inconsistent style | Specify one dominant style: "Professional photography ONLY. No filters." |
| Character drifts across images | Supply reference image + "Character appearance is LOCKED to Image 1" |
| Translation looks wrong | Add: "Match original font weight, match text box size, do not rearrange layout" |
| Need accurate factual visual | Use Nano Banana 2.1 or Pro with "use real, current data" - triggers Search Grounding |
| 4K image too expensive | Generate at 2K, upscale offline; or use Nano Banana 2.1 Batch at $0.0567 per 4K image |
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
- **Picking Pro for thumbnails** - Pro is for high-fidelity work; thumbnails don't need it and pay about 4x more per image at 1K
- **Emitting `-preview` model IDs** - Google lists all of them as shut down. For new work use `gemini-nano-banana-2.1`, `gemini-3.1-flash-lite-image` or `gemini-3-pro-image`. `gemini-3.1-flash-image` and `gemini-2.5-flash-image` still run but are deprecated
- **Building anything new on `gemini-3.1-flash-image`** - it is deprecated, and Google recommends Nano Banana 2.1 for all new projects
- **Reaching for Lite on a hard job** - many references, long edit chains, grounded facts, or anything above 1K all rule it out
- **Setting the canvas in prose only** - "make it 16:9" without `aspect_ratio` gives you a square or an input-matched size

---

## Quick Reference Card

**Always include:**
- Subject + action + setting
- Lighting conditions
- Art style / medium
- Technical specs (resolution, aspect ratio)
- What to preserve (for edits)

**Model picks:**
- Default for anything → **Nano Banana 2.1** (`gemini-nano-banana-2.1`)
- Thumbnail from a finished video → **Nano Banana 2.1** (video input also on Lite, not on Pro)
- Banner at `4:1`, `8:1`, `1:4`, `1:8` → **Nano Banana 2.1**
- Interactive edit loops, sub-2s latency, lowest input cost → **Nano Banana 2 Lite** (`gemini-3.1-flash-lite-image`)
- Studio-quality design / dense accurate text / brand & factual precision → **Nano Banana Pro** (`gemini-3-pro-image`)
- Code on `gemini-3.1-flash-image` → migrate to Nano Banana 2.1, no shutdown date yet
- Legacy code on `gemini-2.5-flash-image` → migrate to Lite before **2027-03-15**

For ready-to-use prompts by category, see `references/prompt-examples.md`. For pricing breakdown by tier, see `references/pricing.md`.

---

## Verification & Sources

Every numeric claim, model ID, capability flag, and price in this skill is sourced from Google's official Gemini API docs, verified on **2026-11-06**.

The docs moved from `ai.google.dev/gemini-api/docs/` to `aistudio.google.com/docs/`; old URLs redirect with a 301. Raw markdown is still served at `ai.google.dev/gemini-api/docs/<page>.md.txt`, and it is the version to read: the rendered deprecations page collapses its model tables, including the Nano Banana one, so a headless-browser dump misses them.

- **Image generation guide**: https://aistudio.google.com/docs/image-generation
- **Nano Banana 2.1 card** (`gemini-nano-banana-2.1`): https://aistudio.google.com/docs/models/gemini-nano-banana-2.1
- **Nano Banana 2 card** (`gemini-3.1-flash-image`): https://aistudio.google.com/docs/models/gemini-3.1-flash-image
- **Nano Banana 2 Lite card** (`gemini-3.1-flash-lite-image`): https://aistudio.google.com/docs/models/gemini-3.1-flash-lite-image
- **Nano Banana Pro card** (`gemini-3-pro-image`): https://aistudio.google.com/docs/models/gemini-3-pro-image
- **Nano Banana legacy card** (`gemini-2.5-flash-image`): https://aistudio.google.com/docs/models/gemini-2.5-flash-image
- **Pricing page**: https://aistudio.google.com/docs/pricing
- **API release notes** (GA dates, deprecations, sampling params): https://aistudio.google.com/docs/changelog
- **Deprecations and shutdown dates**: https://aistudio.google.com/docs/deprecations
- **SynthID watermark**: https://ai.google.dev/responsible/docs/safeguards/synthid
- **Video generation** (Veo shutdown, Gemini Omni Flash): https://aistudio.google.com/docs/video

Key dates from those sources, worth re-checking on each audit:

| Date | Event |
|---|---|
| 2026-05-28 | `gemini-3.1-flash-image` + `gemini-3-pro-image` GA; video-to-image launched |
| 2026-06-25 | `-preview` image model IDs listed as shut down |
| 2026-06-30 | `gemini-3.1-flash-lite-image` (Nano Banana 2 Lite) GA |
| 2026-07-21 | `temperature` / `top_p` / `top_k` deprecated API-wide |
| 2026-08-17 | Imagen 4 models shut down |
| 2026-10-06 | `gemini-nano-banana-2.1` GA; `gemini-3.1-flash-image` deprecated, no shutdown date |
| 2026-10-22 | Veo 3.1 Preview shut down in the Gemini API |
| 2027-03-15 | `gemini-2.5-flash-image` shutdown |

Things this skill **does not** claim because the official docs do not confirm them, or contradict themselves:
- A "100+ languages" count for text rendering. The docs list 15 best-performance locales (Polish not among them) and say nothing about a total.
- C2PA on any model except Nano Banana 2 Lite, whose card names it explicitly.
- That `-preview` IDs fail or work. The deprecations page lists them as shut down, the API model list still returned them on 2026-11-06, and nobody tested a generation call.
- Anything about `nano-banana-pro-preview`. It appears in the API model list and nowhere in the docs.
- Extreme ratios on Lite. Its card headline says "14 aspect ratios" but it lists 10, and the guide's size tables have no Lite tab.
- Tokens per 4K image on Nano Banana 2.1. The pricing page says 3780 ($0.113), the guide's table says 2520. The dollar figures here follow the pricing page.
- Pro's exact high-fidelity count beyond the table. The reference-image table says up to 6 objects, 5 characters and 3 style images; the Limitations list still says "5 images with high fidelity".
- Batch prices for Pro and the deprecated Nano Banana 2. Their pricing sections show only a Standard tab, while their cards list Batch as supported and the Batch API page promises 50% of standard cost in general.
- Default model claims for the consumer Gemini app - this skill targets the API.

If you discover a claim above is wrong, the fix is to update both this section and the relevant body section with the corrected fact and its source URL.
