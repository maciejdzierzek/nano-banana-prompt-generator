# Nano Banana Pro Prompt Generator - Claude Skill

> A Claude skill for generating professional Nano Banana image prompts.
> Works with Claude.ai, Claude Desktop, and Claude Code.
> Supports Image Generation, Photo Editing, YouTube Thumbnails, and Infographics.

[![Live Generator](https://img.shields.io/badge/Live_Generator-maciejdzierzek.com-blue)](https://maciejdzierzek.com/narzedzia/generator-nano-banana)
[![Claude Skill](https://img.shields.io/badge/Claude-Skill-orange)](https://claude.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## What is this?

A skill for Claude that helps you write effective prompts for Nano Banana - Google's family of AI image generation models built into Gemini. Unlike most AI image tools that expect tag lists, Nano Banana works like a creative director: it understands intent, context, and full-sentence descriptions.

Works with **Claude.ai** (web), **Claude Desktop**, and **Claude Code** - all three support the same skill format via the Customize > Skills panel.

The current default model is **Nano Banana 2** (Gemini 3.1 Flash Image), combining Pro-quality reasoning with Flash speed. It supports text rendering in 100+ languages, multi-character consistency (up to 5 characters), up to 14-object fidelity, 4K resolution, and integration with Veo 3.1 for animation pipelines.

This skill was built by [Maciej Dzierżek](https://maciejdzierzek.com) and used daily in production workflows across image generation, content creation, and YouTube thumbnail design.

**Try the interactive web version:** [maciejdzierzek.com/narzedzia/generator-nano-banana](https://maciejdzierzek.com/narzedzia/generator-nano-banana)

## Features

- **4 generation modes:** Image Generation, Photo Editing, YouTube Thumbnails, Infographics
- Text rendering in 100+ languages - render Polish headlines, Japanese labels, Arabic text
- In-image translation (new in 2026) - translate text inside existing images while preserving design
- Multi-character consistency - up to 5 characters across a workflow
- Multi-reference compositions - up to 14 objects with high fidelity
- Conversational iterative editing - refine images without starting from scratch
- Real-world knowledge via Search grounding for accurate diagrams and data visualizations
- Video production pipeline support - generate storyboard keyframes for Veo 3.1 handoff
- Golden Rules for prompting - no tag soup, no vague descriptions, no wasted retries
- Ready-to-use templates for every category

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
/plugin install nano-banana-prompt-generator@maciejdzierzek-nano-banana-prompt-generator
```

No download needed - installs directly from this GitHub repository.

### Claude Code (CLI) - manual install

```bash
mkdir -p ~/.claude/skills/nano-banana
cp skills/nano-banana/SKILL.md ~/.claude/skills/nano-banana/SKILL.md
```

For project-specific use:
```bash
mkdir -p /your-project/.claude/skills/nano-banana
cp skills/nano-banana/SKILL.md /your-project/.claude/skills/nano-banana/SKILL.md
```

Reload Claude Code. The skill activates automatically.

### Interactive web generator (no installation)

Use directly at: [maciejdzierzek.com/narzedzia/generator-nano-banana](https://maciejdzierzek.com/narzedzia/generator-nano-banana)
No API, no registration, no data sent to servers. Works in your browser.

## Usage

Describe what you want to create - the skill adapts to all four modes:

**Image Generation:**
```
Generate a Nano Banana prompt for a product photo of my coffee grinder.
I want a minimalist studio look, white background, space for text on the right.
```

**Photo Editing:**
```
I have a photo of a storefront in summer. Write a Nano Banana prompt
to transform it into a winter scene while keeping the building exactly the same.
```

**YouTube Thumbnail:**
```
I need a thumbnail for my iPhone review video. I want the excited-person-on-left,
product-on-right layout. Help me write it.
```

**Infographic:**
```
Create a Nano Banana prompt for a comparison infographic: Agency vs In-house AI team.
4 advantages on each side, square format for LinkedIn.
```

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
[Style/quality requirements]. [Technical specs].
```

### YouTube Thumbnail

```
YouTube thumbnail for [TOPIC]:

SUBJECT: [person description + expression + position in frame]
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

See [examples/](examples/) for ready-to-use prompts with expected output notes:

- [generation.md](examples/generation.md) - product photography, portraits, cinematic scenes
- [editing.md](examples/editing.md) - object removal, season/style changes, text localization
- [youtube-thumbnails.md](examples/youtube-thumbnails.md) - tech review, before/after, tutorial layouts
- [infographics.md](examples/infographics.md) - timeline, comparison, statistical dashboard

For the complete template library, see [references/prompt-examples.md](references/prompt-examples.md).

## Golden Rules

These rules consistently produce better results with Nano Banana 2:

1. **Creative Director, not keyword vomit** - Write full sentences describing a scene, not tag lists like `dog, park, 4k, realistic, sunny`
2. **Edit, don't re-roll** - If an image is 80% right, ask for specific changes instead of regenerating from scratch
3. **Explain the logic** - The model understands intent. Say *why*, not just *what*: "as if it were a recipe, with sunlight as an ingredient"
4. **Specify preservation explicitly** - Always state what must not change during edits with a `KEEP UNCHANGED:` or `CRITICAL:` section
5. **Provide context and purpose** - "For a Brazilian gourmet cookbook" helps the model make better artistic decisions
6. **Use positive framing** - Describe what you want, not what you don't want
7. **Specify HEX codes for brand colors** - "background color: #0066CC" is more reliable than "dark blue"

## Model Overview

| Model | Gemini Base | Best For | Speed |
|---|---|---|---|
| **Nano Banana 2** | Gemini 3.1 Flash Image | Default - all-purpose, Pro quality + Flash speed | Fast |
| **Nano Banana Pro** | Gemini 3.1 Pro Preview | Highest-fidelity, most demanding outputs | Slower |
| Nano Banana (original) | Gemini 2.5 Flash | Quick simple edits | Fastest |

**Default recommendation:** Use Nano Banana 2 for almost everything. Upgrade to Pro only for the most demanding professional outputs. The original Gemini 3 Pro Image was deprecated March 9, 2026.

**Access:** Gemini app, Google Flow, AI Studio, Vertex AI, Gemini API, Gemini CLI

## About the Author

Built by [Maciej Dzierżek](https://maciejdzierzek.com) - consultant, trainer and creator from Poland. He specializes in helping businesses implement AI in their day-to-day workflows, and built this skill to solve his own real-world prompting problems with Nano Banana.

- Website: [maciejdzierzek.com](https://maciejdzierzek.com)
- All AI tools: [maciejdzierzek.com/narzedzia](https://maciejdzierzek.com/narzedzia)

## License

MIT - use freely, attribution appreciated.
