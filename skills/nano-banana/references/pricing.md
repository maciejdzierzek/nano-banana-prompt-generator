# Nano Banana / Gemini Image Generation Pricing

**Verified against the official Gemini API pricing page** at [ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing) on **2026-05-21**. Pricing changes - verify in the Gemini API dashboard before quoting figures to anyone.

All prices are USD, per the official paid tier. Image-generation models do **not** have a free tier - the Free Tier column shows "Not available" for all three.

---

## Per-image output cost (Standard tier)

| Model | 0.5K (512px) | 1K (1024px) | 2K (2048px) | 4K (4096px) |
|---|---|---|---|---|
| Nano Banana (`gemini-2.5-flash-image`) | - | $0.039 | - | - |
| Nano Banana 2 (`gemini-3.1-flash-image-preview`) | $0.045 | $0.067 | $0.101 | $0.151 |
| Nano Banana Pro (`gemini-3-pro-image-preview`) | - | $0.134 | $0.134 | $0.24 |

**Token math (verified in pricing footnotes):**
- Nano Banana 2 image output: $60 / 1M tokens. 0.5K=747 tokens, 1K=1120 tokens, 2K=1680 tokens, 4K=2520 tokens.
- Nano Banana Pro image output: $120 / 1M tokens. 1K-2K=1120 tokens ($0.134), 4K=2000 tokens ($0.24).
- Nano Banana (2.5): $30 / 1M tokens. 1024x1024 = 1290 tokens ($0.039).

---

## Input cost (per 1M tokens, Standard tier)

| Model | Text/image input |
|---|---|
| Nano Banana | $0.30 |
| Nano Banana 2 | $0.50 |
| Nano Banana Pro | $2.00 (text/image), equivalent to **$0.0011 per input image** (560 tokens flat) |

---

## Batch API (50% discount)

Batch tier halves both input and output prices, with up to 24h turnaround:

| Model | 1K image output (Batch) | 4K image output (Batch) |
|---|---|---|
| Nano Banana | $0.0195 | - |
| Nano Banana 2 | $0.034 | $0.076 |
| Nano Banana Pro | $0.067 | $0.12 |

Use Batch for bulk generation jobs where latency doesn't matter (e.g., generating 100 thumbnail variants).

---

## Flex and Priority tiers

- **Flex** (Pro and Nano Banana only): same pricing as Batch on Pro ($0.067 per 1K-2K image, $0.12 per 4K). On `gemini-2.5-flash-image`: $0.0195 per image. Lower priority than Standard, occasional 503s under load.
- **Priority** (all three models): roughly 1.8x Standard cost in exchange for higher rate limits and lower latency. Nano Banana Priority: $0.0702/image. Nano Banana 2 Priority: not separately listed in current docs. Pro Priority: $216 per 1M output tokens.

For most workflows: use **Standard**. Bulk jobs: **Batch**. Real-time / interactive apps: **Priority**.

---

## Search Grounding (Nano Banana 2 and Pro)

When a generation triggers a Google Search query for grounding:
- **5,000 prompts per month free** (shared across all Gemini 3 family models)
- **$14 / 1,000 search queries** above the free quota

Image Search Grounding (Nano Banana 2 only) is billed the same way as Web Search.

Original Nano Banana (2.5 Flash Image) does **not** support search grounding.

---

## What's NOT charged

- **Thinking tokens** for "thought images" (Nano Banana 2 and Pro generate interim images while reasoning - these are visible in the backend but not billed). Source: image-generation guide.
- **SynthID watermarking** - automatic, no charge.
- **Retrieved grounding context** (text or images Google Search returns to the model) - not counted as input tokens. Only the search query itself is billed.

---

## Free playgrounds

- **Google AI Studio** (`aistudio.google.com`) - free interactive UI for all three models. Counts against per-account quotas, not free billing tokens.
- **Gemini consumer app** - free, uses unspecified default model (likely `gemini-2.5-flash-image` or successor). Not part of API quota.

These free surfaces are useful for prompt iteration. Once you switch to the API for production, pricing kicks in immediately.

---

## Rough cost guidance

| Scenario | Recommended model | Approx cost per output |
|---|---|---|
| 100 product photos for a catalog | Nano Banana (Standard) or Nano Banana 2 (Batch) | $3.90 - $3.40 |
| 50 YouTube thumbnails with bold text | Nano Banana 2 (Standard, 1K) | $3.35 |
| 10 high-fidelity infographics with accurate data | Nano Banana Pro (Standard, 2K) | $1.34 |
| 1 hero image for landing page (4K) | Nano Banana Pro (Standard, 4K) | $0.24 |
| Conversational editing session (5 turns) | Nano Banana (Standard, 1K) | ~$0.20 |

---

## Source

- Official pricing page: https://ai.google.dev/gemini-api/docs/pricing
- Per-model token math is in footnotes on the same page (sections for `gemini-3.1-flash-image-preview`, `gemini-3-pro-image-preview`, `gemini-2.5-flash-image`).
- Preview models (Nano Banana 2 and Pro) may change pricing before reaching Stable status.
