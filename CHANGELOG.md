# Changelog

All notable changes to the Nano Banana Prompt Generator skill and plugin are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0] - 2026-11-06

### Added

- **Nano Banana 2.1** (`gemini-nano-banana-2.1`, GA 2026-10-06) as the default model, with 1:4, 4:1, 1:8 and 8:1 ratios and a banner prompt pattern.

### Changed

- `gemini-3.1-flash-image` marked as deprecated, no shutdown date. Migration notes: no 0.5K, thinking defaults to `medium`, cheaper image but dearer input and text tokens.
- Docs links moved to `aistudio.google.com/docs`.

### Fixed

- `gemini-2.5-flash-image` shuts down on 2027-03-15, not 2026-10-02.
- Lite is not half the price: at 1K the image costs the same as 2.1, Lite only wins on input and text tokens.
- Video input is not exclusive to one model. Corrected object and character limits for Pro, and capability flags for Lite, legacy and Pro.
- Veo 3.1 Preview left the Gemini API on 2026-10-22, so the storyboard section was rewritten.

## [1.2.0] - 2026-08-05

Maintenance audit against Google's official Gemini API docs (`ai.google.dev/gemini-api/docs`, fetched from the canonical `.md.txt` sources on 2026-08-04). **This is a correctness-critical release: the model IDs shipped in v1.1.0 have been shut down by Google.**

### Fixed

- **Dead model IDs removed.** `gemini-3.1-flash-image-preview` and `gemini-3-pro-image-preview` - both recommended by v1.1.0 - were **shut down on 2026-06-25**. Replaced with the GA IDs `gemini-3.1-flash-image` and `gemini-3-pro-image` (both GA 2026-05-28). A skill that emits a shut-down model ID produces a 404 for the user; this was the reason for the audit.
- **Default model recommendation reversed.** v1.1.0 recommended `gemini-2.5-flash-image` as "the only stable model" and the production default. That model is now **deprecated with shutdown on 2026-10-02**, and Google's own guidance is to migrate off it. The default is now Nano Banana 2 (`gemini-3.1-flash-image`), matching Google's "should be your go-to image generation model" wording in the image-generation guide.
- **"Preview" framing dropped throughout.** All four models are Stable/GA; the caveats about preview instability and restrictive preview rate limits no longer apply.
- **C2PA claim corrected.** v1.1.0 stated the docs never mention C2PA. Nano Banana 2 Lite's model card explicitly lists "SynthID (Always On) + C2PA". The claim is now per-model instead of blanket-denied.
- **Pro reference-image capacity corrected** from "up to 6 objects high-fidelity" to the documented 5, and the invented per-character count for Pro removed (Google does not publish one).
- **Text-rendering language claim replaced with the official list.** v1.1.0 said the docs do not enumerate languages. They now do: 15 best-performance locales. **Polish is not among them** - the skill states this plainly and gives the proof-render / text-layer-in-post fallback, which matters for Polish-language deliverables.

### Added

- **Fourth model: Nano Banana 2 Lite** (`gemini-3.1-flash-lite-image`, GA 2026-06-30) - sub-2s latency, cheapest per image ($0.0336 at 1K Standard, ~$0.017 Batch), 1K only, 10 aspect ratios, function calling supported, no search grounding, explicitly not optimised for multiple references or long multi-turn edit chains.
- **Video-to-image section** - `gemini-3.1-flash-image` accepts a public YouTube URL or an uploaded video file as multimodal context and generates a still from it (launched 2026-05-28, exclusive to this model). Includes thumbnail / poster / summary-infographic prompt patterns and the caveat that output is synthesised art direction, not a frame extraction.
- **Two-step text workflow** from the official best-practices list: generate the copy in one turn, then ask for the image with that copy - instead of one shot.
- **Deprecation timeline table** in "Verification & Sources": GA dates, `-preview` shutdown, Lite GA, sampling-parameter deprecation, Imagen shutdown, legacy 2.5 shutdown. Gives the next audit a dated baseline.
- **`response_format` mechanics** - `aspect_ratio` and `image_size` are parameters, not prose. Documented with the note that stating "16:9" in prompt text does not set the canvas.
- **Grounding limitation** - on Nano Banana 2, Search grounding does not support real-world images of people from web search.
- **Exact pixel dimensions** for common 1K ratios on Nano Banana 2 (16:9 = 1376x768, 9:16 = 768x1376, 4:5 = 928x1152, 1:1 = 1024x1024).
- **4 new eval cases** covering video-to-image, model/cost selection, the 404-on-preview-ID scenario, and Polish diacritics.

### Changed

- **Frontmatter split into `description` + `when_to_use`** (the `when_to_use` field is now supported and is appended to `description` in the skill listing, sharing a 1,536-character cap). Trigger phrases and model codes moved out of `description`, which now states what the skill does. Combined length 1,019 characters.
- **Capability matrix rebuilt as four columns** and extended with video input, multi-reference capacity, function calling, Flex/Priority availability, and token limits, all from the per-model cards.
- **Resolution and aspect-ratio tables** now per-model (Lite is 1K-only; the extreme banner ratios `1:4`/`4:1`/`1:8`/`8:1` are exclusive to Nano Banana 2).
- **`references/pricing.md` rewritten** for four models across Standard / Batch / Flex / Priority, with the note that Flex and Priority are unavailable on Nano Banana 2 and that legacy 2.5 costs more than Lite for a worse result.
- **Sampling parameters** - noted `temperature`, `top_p`, `top_k` were deprecated API-wide on 2026-07-21, so prompt advice must not lean on them.
- **Access points** - the image docs are now written against the Interactions API (`client.interactions.create`, `response_format`) rather than `generate_content`.

### Source verification notes

Every fact above traces to one of: the image-generation guide, the four model cards, the pricing page, the API changelog, or the deprecations page - all on `ai.google.dev`, fetched 2026-08-04 as raw markdown. No third-party reviews were used. Where Google does not publish a number (Pro character count, total supported language count, C2PA on models other than Lite), the skill says so instead of estimating.

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
