# Changelog

All notable changes to the Nano Banana Prompt Generator skill and plugin are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-05-21

This release was rewritten against Google's **official Gemini API documentation** at [ai.google.dev/gemini-api/docs](https://ai.google.dev/gemini-api/docs), fetched via `curl` directly from the canonical markdown sources on 2026-05-21. The earlier draft relied on third-party reviews and contained factual errors corrected below. All facts in this entry are sourced from official Google pages - URLs cited in `SKILL.md` "Verification & Sources" section.

### Added

- **Three-model lineup with canonical model IDs** - `gemini-2.5-flash-image` (Nano Banana, Stable), `gemini-3.1-flash-image-preview` (Nano Banana 2, Preview), `gemini-3-pro-image-preview` (Nano Banana Pro, Preview). Each row includes status, last-updated date, and best-use guidance.
- **Per-model capability matrix** - thinking, search grounding, batch, caching, structured outputs - sourced from each model's spec page on ai.google.dev.
- **Image Search Grounding** section - documented for Nano Banana 2 (3.1 Flash adds Image Search alongside Web Search per the model spec).
- **`references/pricing.md`** - per-image cost across Standard / Batch / Flex / Priority tiers, plus token math footnotes copied verbatim from the official pricing page. Verified 2026-05-21.
- **`evals/evals.json`** - 12 should-trigger + 7 should-not-trigger + 2 edge cases for the skill-creator framework.
- **`.claude-plugin/marketplace.json`** - the repo now serves as both plugin source and marketplace catalog. Marketplace name: `maciejdzierzek` (matches kling-ai marketplace for cross-skill consistency).
- **Verification & Sources section** in SKILL.md - links every major claim back to its docs URL on ai.google.dev.
- **Explicit "what this skill does NOT claim" section** - calls out unverified numbers (the "100+ languages" figure, C2PA, the 4-stage Veo pipeline) so future authors do not reintroduce them.
- **Storyboards for Video Workflows** section - documents the actual handoff to Veo, referencing the Veo guide instead of inventing a multi-stage pipeline.
- **CHANGELOG.md** (this file).

### Changed

- **Frontmatter `description`** - extended to cover all three models and feature surfaces (in-image translation, search grounding, multi-character consistency limits). Length within character cap.
- **Quick Start** example switched from "4K" default to "2K, 3:2" to match a realistic billing decision (4K is 2.3x more expensive than 2K on Nano Banana 2; thumbnail use cases need 1K).
- **Marketplace install command** standardized to `/plugin install nano-banana-prompt-generator@maciejdzierzek` - shorter marketplace name, consistent with kling-ai-prompt-generator.
- **plugin.json description** updated to mention all three model IDs and key features.
- **README.md** - Features list rewritten, Models table refreshed against per-model spec pages, install commands corrected.
- **Aspect ratio list** in technical specs - corrected from the partial 4 ratios in the earlier draft to the full 14 documented in the image-generation guide (`1:1, 1:4, 1:8, 2:3, 3:2, 3:4, 4:1, 4:3, 4:5, 5:4, 8:1, 9:16, 16:9, 21:9`).
- **Capacity claims** split per-model. Earlier draft mixed Pro and NB2 capacity into a single misleading row. Now each model has documented limits sourced from the "Use up to 14 reference images" table in the image-generation guide.

### Fixed (corrections from official source verification)

- **Nano Banana Pro is `gemini-3-pro-image-preview`, NOT `gemini-3.1-pro-preview`.** Earlier draft conflated the image-generation Pro model with the text/multimodal Gemini 3.1 Pro Preview - these are different products on different pricing tiers. The image Pro model is `gemini-3-pro-image-preview` per its own spec page.
- **Nano Banana (original) is the Stable mainstream model, not "Simple quick edits".** Per the `gemini-2.5-flash-image` spec page: "best for high-volume generation, conversational image editing, and low-latency creative workflows that require native multimodal understanding." Repositioned accordingly in the Model Lineup table.
- **"Default: Nano Banana 2" softened.** NB2 is a Preview model and may change; production defaults should be `gemini-2.5-flash-image` (Stable). The skill now distinguishes "production default" from "feature-richer Preview option".
- **Multi-character / multi-object capacity per model** corrected. Earlier draft claimed "up to 14 reference images, 6 with high fidelity" with mixed-model attribution. Actual per-model numbers from the official "Use up to 14 reference images" table:
  - Nano Banana 2: up to 10 objects high-fidelity, up to 4 characters consistency, 14 total
  - Nano Banana Pro: up to 6 objects high-fidelity, up to 5 characters consistency, 14 total
  - Original Nano Banana: best with up to 3 input images
- **Aspect ratios full list** - was 4 ratios, actual is 14 per the Python code example in the image-generation guide.
- **Per-image pricing** - added verified token math for each model. Previously omitted; now sourced from pricing-page footnotes.

### Removed

- **"Supported for 100+ languages"** - third-party claim. Official docs do not enumerate a language count. The Spanish example in the in-image-translation section is the only language-coverage hint in the guide. Replaced with "verify against current docs if a specific script matters."
- **"SynthID + C2PA Content Credentials"** watermark claim - C2PA is not mentioned in the Gemini API image-generation docs. Only SynthID is documented. C2PA reference removed entirely.
- **"4-stage video production pipeline: Nano Banana + Veo + Lyria + Flow"** - third-party narrative. Official docs only link to the Veo guide at the end of the image-generation page. The "keyframe handoff" prompt template remains (as `Storyboards for Video Workflows`) but is no longer presented as a documented Google pipeline.
- **"Default access via Gemini app, Flow, AI Studio, Vertex AI, Gemini API, Gemini CLI"** - access list trimmed to surfaces explicitly named in the API docs (Gemini API, Google AI Studio, Vertex AI). Consumer surfaces mentioned in passing but no longer presented as exhaustive.
- **"Gemini 3 Pro Image was deprecated March 9, 2026"** (was in README) - false. `gemini-3-pro-image-preview` is the current Preview Pro model and remains active.
- Stale `version: 1.0.0` placeholder.

### Source verification notes

This release was researched against:
- https://ai.google.dev/gemini-api/docs/image-generation - main guide
- https://ai.google.dev/gemini-api/docs/models/gemini-2.5-flash-image - Nano Banana spec
- https://ai.google.dev/gemini-api/docs/models/gemini-3.1-flash-image-preview - Nano Banana 2 spec
- https://ai.google.dev/gemini-api/docs/models/gemini-3-pro-image-preview - Nano Banana Pro spec
- https://ai.google.dev/gemini-api/docs/pricing - per-model and per-tier pricing

Fetched via `curl https://...md.txt` from the canonical Devsite markdown sources, not via WebFetch summaries.

Things NOT verified and therefore softened or omitted:
- The "100+ languages" text-rendering count (no enumeration in official docs - skill says "improved i18n text rendering" per the 3.1 Flash release blurb).
- C2PA support (not mentioned in image-generation docs - skill says SynthID only).
- Default consumer-app model (skill scopes to the API surface).
- Lyria audio integration with image generation (not documented as a connected workflow).

Verify against ai.google.dev/gemini-api/docs before quoting any specific number to a client.

## [1.0.0] - 2026-03-14

### Added

- Initial release of the Nano Banana prompt generator skill.
- SKILL.md with model overview (3 models), prompt structures for generation/editing/thumbnails/infographics, Golden Rules, technical specs, common issues.
- `references/prompt-examples.md` - template library by category.
- `examples/` directory - generation, editing, thumbnails, infographics samples.
- Plugin manifest (`plugin.json`), MIT license, README with installation instructions for Claude.ai, Claude Desktop, and Claude Code.

Source basis: third-party reviews and community discussion (no canonical Google docs verification at the time). Corrections incorporated in v1.1.0.
