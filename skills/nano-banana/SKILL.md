---
name: nano-banana
description: This skill should be used when the user wants to generate images, edit photos, design YouTube thumbnails, create infographics, build storyboards, generate Veo video keyframes, or translate text inside images - all using Nano Banana (Google's Gemini image models). Applies to any request involving Nano Banana, Gemini image generation, AI photo editing, AI-generated thumbnails, infographic design, storyboard creation, or multi-character image consistency. Also covers casual requests such as "help me make an image" or "can Gemini edit this photo".
---

# Nano Banana Prompting Guide (2026)

## Model Landscape

Nano Banana is Google's family of AI image generation and editing models built into Gemini.

| Model | Gemini Base | Best For | Speed |
|---|---|---|---|
| **Nano Banana 2** | Gemini 3.1 Flash Image | Default - all-purpose, Pro quality + Flash speed | Fast |
| **Nano Banana Pro** (Gemini 3.1 Pro Preview) | Gemini 3.1 Pro | Highest-fidelity, complex reasoning tasks | Slower |
| Nano Banana (original) | Gemini 2.5 Flash | Simple quick edits | Fastest |

**Default recommendation:** Use Nano Banana 2 for almost everything. Upgrade to Pro only for the most demanding professional outputs.

---

## Quick Start: Generate or Edit an Image in 60 Seconds

**Generate:** Describe your scene in a full sentence with style and specs:
```
A professional product photo of a black leather wallet on white marble.
Soft studio lighting, macro lens, shallow depth of field. 4K, 3:2 aspect ratio.
```

**Edit:** Stay in the same conversation and describe only what changes:
```
Turn 1: "A red convertible parked on a coastal road at sunset."
Turn 2: "Change the color to midnight blue."
Turn 3: "Add surfboards on the roof rack."
```
The model remembers the full visual context - no need to re-describe unchanged elements.

For the full prompt formula, model details, and advanced techniques - read on.

---

## Language of Prompts

Always write Nano Banana prompts in **English**, regardless of the language the user is writing in. The model was trained predominantly on English-language descriptions and produces significantly better results with English prompts.

The workflow is:
- Communicate with the user in their language (Polish, German, French, etc.)
- Write all image generation prompts in English

If the user describes what they want in Polish or another language, translate it to English before presenting the final prompt they should use. Explain this briefly if it's not obvious.

Exception: when the explicit goal is to render non-English text *inside the image* (e.g., a Polish poster headline, a Japanese label), that text stays in the target language - but the surrounding prompt instructions remain in English.

---

## Golden Rules

1. **Creative Director, not keyword vomit** - Write full sentences describing a scene, not tag lists.
2. **Edit, don't re-roll** - If an image is 80% right, ask for specific changes. Preserve the 80% that works.
3. **Explain the logic** - Nano Banana 2 is a thinking model. It understands intent. Say *why*, not just *what*: "Create a diagram of photosynthesis as if it were a recipe, with sunlight as an ingredient."
4. **Specify preservation explicitly** - Always state what must not change during edits.
5. **Provide context and purpose** - "For a Brazilian gourmet cookbook" helps the model make better artistic decisions.
6. **Use positive framing** - Describe what you want, not what you don't want.

---

## Core Prompt Structure (6 Factors)

Every strong Nano Banana prompt covers these six factors:

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
4K, 16:9, shallow depth of field.
```

### Image Editing Template

```
[Action verb] + [specific element] + [desired result].
KEEP UNCHANGED: [list of what must stay the same].
[Style/quality requirements]. [Technical specs].
```

**Action verbs:** Replace, Remove, Add, Change, Transform, Colorize, Translate, Enhance

**Example:**
```
Replace the white top with a black t-shirt.
KEEP UNCHANGED: face, hairstyle, pose, background, lighting.
Photorealistic, natural shadows. High resolution, seamless integration.
```

---

## Reference Images (Role Assignment)

Nano Banana 2 supports up to 14 reference images with high fidelity for up to 6. Assign each reference a specific role:

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
Photorealistic. 4K, 3:2 aspect ratio.
```

---

## Multi-Character and Object Consistency

Nano Banana 2 can maintain:
- Up to **5 characters** with visual consistency across a workflow
- Up to **14 objects** with fidelity in a single generation

### Multi-character prompt pattern:
```
Scene features [CHARACTER A] (tall woman, red coat, short dark hair)
and [CHARACTER B] (older man, grey beard, blue jacket).
Keep both characters visually identical to the reference images.
[Scene description, action, setting, composition]
```

### Sequential consistency (storyboarding):
```
Continue this story from the previous image. The main character -
[detailed description or "same as Image 1"] - is now doing [new action].
KEEP IDENTICAL: character appearance, clothing, hair, distinguishing features.
CHANGE: [what is different in this scene].
```

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

### In-image translation (new in 2026):
Nano Banana 2 can translate text within existing images while preserving the design:
```
Translate all [SOURCE LANGUAGE] text in this image to [TARGET LANGUAGE].
Keep everything identical: layout, colors, images, design elements.
Match the original font style and weight as closely as possible.
All translated text must be perfectly legible and properly formatted.
```
Supported for 100+ languages.

---

## Conversational Editing

Iterate naturally in the same conversation. The model remembers the full visual context - you describe only what changes, not the whole scene:

```
Turn 1: "A corporate headshot of a woman in a navy suit, white background, studio lighting."
Turn 2: "Change the background to a blurred modern office."
Turn 3: "Add glasses."
Turn 4: "Make the lighting warmer, golden hour feel."
```

Each edit refines the existing image. Use this instead of starting from scratch when only one element needs changing.

---

## Search Grounding (Real-World Knowledge)

Nano Banana 2 connects to Google Search and Gemini's world knowledge for factually accurate visuals:

```
Visualize the current weather forecast for Tokyo next 5 days
as a modern weather chart with temperature values, icons for conditions,
and day labels. Clean professional style, 16:9, 1920x1080px.
```

```
Create an accurate floor plan diagram of a typical Japanese izakaya restaurant.
Show bar seating, tatami rooms, kitchen area, entrance.
Labeled in English. Architectural diagram style, clean white background.
```

---

## Video Production Pipeline (Nano Banana + Veo + Lyria)

Nano Banana 2 is the starting point for full AI video production. The four-stage workflow:

**Stage 1 - Storyboard:** Plan scenes with consistent characters and settings in Nano Banana
**Stage 2 - Keyframe generation:** Create still images for each key moment
**Stage 3 - Video generation:** Hand keyframes to Veo 3.1 to generate motion between them
**Stage 4 - Final production:** Add soundtrack via Lyria, edit in Flow

### Keyframe prompt for Veo handoff:
```
Create a keyframe for an animation scene.
[Subject description with all consistency details]
doing [specific action], in [setting].
[Precise camera angle and framing - e.g. "low angle, wide shot"].
[Lighting that works for animation, e.g. "soft even diffuse light"].
Style: [art direction consistent with other keyframes].
This will be used as a Veo animation keyframe - ensure clear foreground/background separation.
```

---

## YouTube Thumbnails

### Structure:
```
[Subject positioning] + [Expression/action] + [Text overlay specs]
+ [Background treatment] + [Contrast/color requirements] + [Technical specs]
```

### Key principles:
- **High contrast** - minimum 30% contrast between elements for mobile visibility
- **Bold text** - max 3-5 words, 36pt+ minimum, thick fonts with outline or shadow
- **Strong emotion** - excited, surprised expressions increase CTR 20-30%
- **Rule of thirds** - face left, product right
- **Avoid bottom-right** - YouTube timestamp covers this area

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

---

## Infographics

### Best types for Nano Banana 2:
- Timeline infographics
- Comparison charts (side-by-side)
- Process/How-to diagrams
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

---

## Technical Specs (Nano Banana 2)

| Parameter | Value |
|---|---|
| Max reference images | 14 (6 with high fidelity) |
| Resolutions | 512px, 1K, 2K, 4K |
| Aspect ratios | 1:1, 3:2, 16:9, 21:9 |
| Input formats | PNG, JPEG, WebP, HEIC, HEIF |
| Watermarking | SynthID + C2PA Content Credentials |
| Access | Gemini app, Flow, AI Studio, Vertex AI, Gemini API, Gemini CLI |

---

## Common Issues and Fixes

| Problem | Solution |
|---|---|
| Text blurry or misspelled | Add: "All text ultra-sharp, perfectly spelled, high resolution" |
| Face changes during edit | Add: "KEEP UNCHANGED: face must be 100% identical to original" |
| Colors don't match brand | Use HEX codes: "background color: #0066CC" |
| Composition shifts | Add: "Preserve exact composition, framing, and layout" |
| Inconsistent style | Specify one dominant style: "Professional photography ONLY. No filters." |
| Character drifts across images | Supply reference image + "Character appearance is LOCKED to Image 1" |
| Translation looks wrong | Add: "Match original font weight, match text box size, do not rearrange layout" |

---

## What NOT to Do

- **Tag soup**: `dog, park, 4k, realistic, sunny, professional`
- **Vague descriptions**: "nice image", "good quality", "make it better"
- **Cliche boosts**: "trending on artstation, masterpiece, 8k ultra" - the model ignores these
- **Re-rolling from scratch** when 80% is already right
- **Missing preservation instructions** during edits
- **Mixing conflicting styles** in one prompt
- **Over-prompting** - 500+ word prompts confuse rather than clarify
- **Celebrity names** - use descriptive attributes instead ("woman with sharp cheekbones and red hair")

---

## Quick Reference Card

**Always include:**
- Subject + action + setting
- Lighting conditions
- Art style / medium
- Technical specs (resolution, aspect ratio)
- What to preserve (for edits)

**Model strengths (Nano Banana 2):**
- Text rendering in 100+ languages + in-image translation
- Multi-character consistency (up to 5 characters)
- Complex multi-reference compositions (up to 14 objects)
- Real-time world knowledge and Search grounding
- Conversational iterative editing
- Native 4K output
- Storyboard and video production pipelines (with Veo + Lyria)

For ready-to-use prompts by category, see `references/prompt-examples.md`.
