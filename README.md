# Nano Banana Prompt Generator - Claude Skill

> A Claude skill for generating professional Nano Banana / Gemini image prompts.
> Works with Claude.ai, Claude Desktop, and Claude Code.
> Supports Image Generation, Photo Editing, YouTube Thumbnails, Infographics, Storyboards.

[![Live Generator](https://img.shields.io/badge/Live_Generator-maciejdzierzek.com-blue)](https://maciejdzierzek.com/narzedzia/generator-promptow/?model=nano-banana)
[![Claude Skill](https://img.shields.io/badge/Claude-Skill-orange)](https://claude.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## What is this?

A skill for Claude that helps you write effective prompts for Nano Banana - Google's family of AI image generation models in the Gemini API. Unlike most AI image tools that expect tag lists, Nano Banana works like a creative director: it understands intent, context, and full-sentence descriptions.

Works with **Claude.ai** (web), **Claude Desktop**, and **Claude Code** - all three support the same skill format.

The skill covers all four current Gemini image models:

| Marketing name | Model code | Status | Niche |
|---|---|---|---|
| **Nano Banana 2** | `gemini-3.1-flash-image` | Stable (GA 2026-05-28) | Default for everything; only model with video input |
| **Nano Banana 2 Lite** | `gemini-3.1-flash-lite-image` | Stable (GA 2026-06-30) | Sub-2s latency, cheapest per image, 1K only |
| **Nano Banana Pro** | `gemini-3-pro-image` | Stable (GA 2026-05-28) | Studio-quality precision, dense accurate text |
| **Nano Banana** (legacy) | `gemini-2.5-flash-image` | ⚠️ Shutdown 2026-10-02 | Migrate off it |

> **Heads-up if you installed v1.1.0:** the `-preview` model IDs it recommended were shut down by Google on 2026-06-25, and the model it set as the production default is being shut down on 2026-10-02. v1.2.0 fixes both. Update the skill.

Every model spec, capability, and price in this skill is verified against [ai.google.dev/gemini-api/docs](https://ai.google.dev/gemini-api/docs). See `SKILL.md` "Verification & Sources" for the full URL list.

**Try the interactive web generator (no install):** [maciejdzierzek.com/narzedzia/generator-promptow](https://maciejdzierzek.com/narzedzia/generator-promptow/?model=nano-banana)

## Features

- **5 generation modes:** Image Generation, Photo Editing, YouTube Thumbnails, Infographics, Storyboards
- Per-model guidance across all four models, including when Lite is the wrong choice
- Conversational editing patterns - refine iteratively without re-rolling
- Multi-character consistency - up to 4 characters (NB2) with documented per-model limits
- Multi-reference compositions - up to 14 reference images with role assignment
- In-image text translation - preserve design while changing language
- Image Search Grounding - pull real, current data into infographics (NB2 and Pro)
- Video-to-image - generate a thumbnail or poster straight from a YouTube URL (NB2 only)
- Official language list for in-image text, and the fallback when your language is not on it (Polish is not)
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
/plugin marketplace add maciejdzierzek/claude-plugins
/plugin install nano-banana-prompt-generator@maciejdzierzek
```

One marketplace, all my skills - see [maciejdzierzek/claude-plugins](https://github.com/maciejdzierzek/claude-plugins). No download needed.

> **If you added this repository as a marketplace before August 2026**, switch to the command above. Claude Code registers one marketplace per name, so the old per-repo catalogues (which all used the same name) replaced each other when you installed more than one skill. This repo's catalogue still resolves, under the name `maciejdzierzek-nano`, but it is no longer the recommended path.

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

Use directly at: [maciejdzierzek.com/narzedzia/generator-promptow](https://maciejdzierzek.com/narzedzia/generator-promptow/?model=nano-banana)
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
| Anything, unless a reason says otherwise | Nano Banana 2 | Google's own go-to pick: 0.5K-4K, web + image grounding, 14 ratios, up to 14 references |
| Thumbnail or poster from a finished video | Nano Banana 2 | The only model that accepts video input |
| Bulk runs, interactive edit loops, tightest budget | Nano Banana 2 Lite | Sub-2s latency, $0.0336 / image at 1K (~$0.017 Batch). 1K only, no grounding |
| Studio-quality work, dense accurate text, brand precision | Nano Banana Pro | Highest fidelity, search-grounded, Thinking on by default, ~2x Nano Banana 2 |

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
8. **Match model to task** - thumbnails on Pro = wasted money; many references or grounded facts on Lite = wrong tool
9. **Never paste a `-preview` model ID** - all of them are shut down; four live IDs only

## About the Author

Built by [Maciej Dzierżek](https://maciejdzierzek.com) - consultant, trainer and creator from Poland. Specializes in helping businesses implement AI in day-to-day workflows.

- Website: [maciejdzierzek.com](https://maciejdzierzek.com)
- All AI tools: [maciejdzierzek.com/narzedzia](https://maciejdzierzek.com/narzedzia/)
- Companion skill (AI video): [kling-ai-prompt-generator](https://github.com/maciejdzierzek/kling-ai-prompt-generator)

## License

MIT - use freely, attribution appreciated.
