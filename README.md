# Nano Banana Prompt Generator - Claude Skill

> A Claude skill for generating professional Nano Banana / Gemini image prompts.
> Works with Claude.ai, Claude Desktop, and Claude Code.
> Supports Image Generation, Photo Editing, YouTube Thumbnails, Infographics, Storyboards.

[![Live Generator](https://img.shields.io/badge/Live_Generator-maciejdzierzek.com-blue)](https://maciejdzierzek.com/narzedzia/generator-nano-banana)
[![Claude Skill](https://img.shields.io/badge/Claude-Skill-orange)](https://claude.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## What is this?

A skill for Claude that helps you write effective prompts for Nano Banana - Google's family of AI image generation models in the Gemini API. Unlike most AI image tools that expect tag lists, Nano Banana works like a creative director: it understands intent, context, and full-sentence descriptions.

Works with **Claude.ai** (web), **Claude Desktop**, and **Claude Code** - all three support the same skill format.

The skill covers all three current Gemini image models:

| Marketing name | Model code | Status |
|---|---|---|
| **Nano Banana** | `gemini-2.5-flash-image` | Stable |
| **Nano Banana 2** | `gemini-3.1-flash-image-preview` | Preview |
| **Nano Banana Pro** | `gemini-3-pro-image-preview` | Preview |

Every model spec, capability, and price in this skill is verified against [ai.google.dev/gemini-api/docs](https://ai.google.dev/gemini-api/docs). See `SKILL.md` "Verification & Sources" for the full URL list.

**Try the interactive web generator (no install):** [maciejdzierzek.com/narzedzia/generator-nano-banana](https://maciejdzierzek.com/narzedzia/generator-nano-banana)

## Features

- **5 generation modes:** Image Generation, Photo Editing, YouTube Thumbnails, Infographics, Storyboards
- Per-model guidance: choose between Stable (Nano Banana) and Preview (NB2, Pro) based on the task
- Conversational editing patterns - refine iteratively without re-rolling
- Multi-character consistency - up to 5 characters (Pro) or 4 (NB2) with documented limits
- Multi-reference compositions - up to 14 reference images with role assignment
- In-image text translation - preserve design while changing language
- Image Search Grounding - pull real, current data into infographics (NB2 and Pro)
- Storyboard frames for Veo handoff - prompt patterns for video-pipeline workflows
- Verified pricing reference - per-tier (Standard / Batch / Flex / Priority) costs
- Trigger accuracy eval suite for the skill-creator framework

## Installation

### Claude.ai and Claude Desktop (all plans, including free)

1. Download **[nano-banana-prompt-generator-skill.zip](nano-banana-prompt-generator-skill.zip)** from this repository (click → Download raw file)
2. In Claude.ai or Claude Desktop: go to **Customize > Skills** → click **"+"** → **"Upload a skill"**
3. Upload the ZIP file and toggle the skill on

Claude will automatically use the skill when you describe a Nano Banana image task. Available on Free, Pro, Max, Team, and Enterprise plans.

> **Note:** Skills require code execution to be enabled. If you don't see the Skills section, go to **Settings > Capabilities** and enable "Code execution and file creation" first.

### Claude Code (CLI) - via plugin marketplace

```bash
/plugin marketplace add maciejdzierzek/nano-banana-prompt-generator
/plugin install nano-banana-prompt-generator@maciejdzierzek
```

No download needed - installs directly from this GitHub repository.

### Claude Code (CLI) - manual install

```bash
mkdir -p ~/.claude/skills/nano-banana
cp -r skills/nano-banana/* ~/.claude/skills/nano-banana/
```

For project-specific use:
```bash
mkdir -p /your-project/.claude/skills/nano-banana
cp -r skills/nano-banana/* /your-project/.claude/skills/nano-banana/
```

Reload Claude Code. The skill activates automatically.

### Interactive web generator (no installation)

Use directly at: [maciejdzierzek.com/narzedzia/generator-nano-banana](https://maciejdzierzek.com/narzedzia/generator-nano-banana)
No API key, no registration, no data sent to servers. Works in your browser.

## Usage

Describe what you want - the skill picks the right model and template:

**Image Generation:**
```
Generate a Nano Banana prompt for a product photo of my coffee grinder.
Minimalist studio look, white background, space for text on the right.
```

**Photo Editing:**
```
I have a photo of a storefront in summer. Write a Nano Banana prompt
to transform it into winter while keeping the building exactly the same.
```

**YouTube Thumbnail:**
```
I need a thumbnail for my iPhone review video. Excited-person-on-left,
product-on-right layout. Help me write it.
```

**Infographic with current data:**
```
Create a Nano Banana Pro prompt for an infographic showing this quarter's
top-5 economies' GDP growth. Use real, current data via search grounding.
```

**Storyboard for Veo:**
```
Write Nano Banana prompts for 6 storyboard frames showing the same character
walking through a forest as the seasons change. Keyframes for Veo handoff.
```

## Model Selection (verified against per-model spec pages)

| Use case | Recommended model | Why |
|---|---|---|
| High-volume generation, conversational editing, lowest cost | Nano Banana | Stable, $0.039 / image, mature |
| Thumbnails with text, current-data infographics, 0.5K-4K range | Nano Banana 2 | Adds thinking, search grounding, 14 aspect ratios, 0.5K resolution |
| Studio-quality work, dense text, professional infographics | Nano Banana Pro | Highest fidelity, 5-character consistency, search-grounded, 3-4x more expensive |

Pricing details: see `skills/nano-banana/references/pricing.md`.

## Prompt Structures

### Image Generation

```
[Subject + adjectives] doing [action] in [setting].
[Composition/camera angle]. [Lighting/atmosphere].
[Style/medium]. [Technical specs: resolution, aspect ratio].
[Constraints or text to render if any]
```

### Photo Editing

```
[Action verb] + [specific element] + [desired result].
KEEP UNCHANGED: [list of what must stay the same].
[Style/quality requirements].
```

### YouTube Thumbnail

```
YouTube thumbnail for [TOPIC]:

SUBJECT: [person + expression + position in frame]
PRODUCT/VISUAL: [what to show on the other side]
TEXT: "[HEADLINE]" [font style, size, color, position]
BACKGROUND: [treatment]

TECHNICAL: 1280x720px, 16:9, [contrast/color notes]
```

### Infographic

```
[Type] infographic: "[TITLE]"

STRUCTURE:
- [Section 1 description]
- [Section 2 description]

DESIGN:
- Colors: Primary [HEX], Secondary [HEX], Background [HEX]
- Typography: [font style], headlines [size]pt

TECHNICAL: [resolution], [aspect ratio], all text ultra-sharp
AVOID: Drop shadows, 3D effects, gradients on charts
```

## Examples

See [examples/](examples/) for ready-to-use prompts:

- [generation.md](examples/generation.md) - product photography, portraits, cinematic scenes
- [editing.md](examples/editing.md) - object removal, season/style changes, text localization
- [youtube-thumbnails.md](examples/youtube-thumbnails.md) - tech review, before/after, tutorial layouts
- [infographics.md](examples/infographics.md) - timeline, comparison, statistical dashboard

Complete template library: [skills/nano-banana/references/prompt-examples.md](skills/nano-banana/references/prompt-examples.md).

## Golden Rules

1. **Creative Director, not keyword vomit** - Write full sentences, not tag lists like `dog, park, 4k, realistic, sunny`
2. **Edit, don't re-roll** - If an image is 80% right, ask for specific changes instead of regenerating
3. **Explain the logic** - The model understands intent. Say *why*, not just *what*
4. **Specify preservation explicitly** - Always state what must not change during edits
5. **Provide context and purpose** - "For a Brazilian gourmet cookbook" helps the model make better artistic decisions
6. **Use positive framing** - Describe what you want, not what you don't want
7. **Specify HEX codes for brand colors** - "background color: #0066CC" is more reliable than "dark blue"
8. **Match model to task** - thumbnails on Pro = wasted money; production defaults on a Preview model = risk

## About the Author

Built by [Maciej Dzierżek](https://maciejdzierzek.com) - consultant, trainer and creator from Poland. Specializes in helping businesses implement AI in day-to-day workflows.

- Website: [maciejdzierzek.com](https://maciejdzierzek.com)
- All AI tools: [maciejdzierzek.com/narzedzia](https://maciejdzierzek.com/narzedzia)
- Companion skill (AI video): [kling-ai-prompt-generator](https://github.com/maciejdzierzek/kling-ai-prompt-generator)

## License

MIT - use freely, attribution appreciated.
