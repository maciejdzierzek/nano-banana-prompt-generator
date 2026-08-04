# Nano Banana / Gemini Image Generation Pricing

**Verified against the official Gemini API pricing page** at [ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing) on **2026-08-04**. Pricing changes - verify in the Gemini API dashboard before quoting figures to anyone.

All prices are USD, paid tier. Image-generation models do **not** have a free tier - the Free Tier column shows "Not available" for all four.

---

## Per-image output cost (Standard tier)

| Model | 0.5K (512px) | 1K (1024px) | 2K (2048px) | 4K (4096px) |
|---|---|---|---|---|
| Nano Banana 2 Lite (`gemini-3.1-flash-lite-image`) | - | **$0.0336** | - | - |
| Nano Banana 2 (`gemini-3.1-flash-image`) | $0.045 | $0.067 | $0.101 | $0.151 |
| Nano Banana Pro (`gemini-3-pro-image`) | - | $0.134 | $0.134 | $0.24 |
| Nano Banana legacy (`gemini-2.5-flash-image`) ⚠️ | - | $0.039 | - | - |

⚠️ Legacy 2.5 is **shut down 2026-10-02** and costs more than Lite for a worse result. There is no cost argument for staying on it.

**Token math (from the official pricing footnotes):**
- Nano Banana 2: image output $60 / 1M tokens. 0.5K = 747 tok, 1K = 1120 tok, 2K = 1680 tok, 4K = 2520 tok. (Note: the aspect-ratio tables list 1120 tok for both 1K and 2K and 2000 tok for 4K - the footnote figures above are the ones used for the dollar equivalents Google publishes.)
- Nano Banana 2 Lite: $30 / 1M tokens. 1K = 1120 tok → $0.0336.
- Nano Banana Pro: $120 / 1M tokens. 1K-2K = 1120 tok ($0.134), 4K = 2000 tok ($0.24).
- Legacy 2.5: $30 / 1M tokens. 1024x1024 = 1290 tok ($0.039).

---

## Input cost (per 1M tokens, Standard tier)

| Model | Text / image input |
|---|---|
| Nano Banana 2 Lite | $0.25 (text/image/video) |
| Nano Banana 2 | $0.50 (text/image) |
| Nano Banana Pro | $2.00 (text/image), equivalent to **$0.0011 per input image** |
| Legacy 2.5 | $0.30 (text/image) |

Text output (and thinking tokens) is billed separately from images: $1.50/1M on Lite, $3/1M on Nano Banana 2, $12/1M on Pro.

---

## Batch API (50% discount, up to 24h turnaround)

| Model | 0.5K | 1K | 2K | 4K |
|---|---|---|---|---|
| Nano Banana 2 Lite | - | $0.0168 | - | - |
| Nano Banana 2 | $0.022 | $0.034 | $0.050 | $0.076 |
| Nano Banana Pro | - | $0.067 | $0.067 | $0.12 |
| Legacy 2.5 | - | $0.0195 | - | - |

Use Batch for bulk generation where latency doesn't matter (e.g. 100 thumbnail variants overnight). At Batch prices Lite lands at **under 2 cents an image**.

---

## Flex and Priority tiers

- **Flex** - available on Pro and legacy 2.5. Same price as Batch (Pro: $0.067 per 1K-2K image, $0.12 per 4K; 2.5: $0.0195). Lower priority than Standard, occasional 503s under load. **Not available on Nano Banana 2.**
- **Priority** - available on Pro and legacy 2.5. Roughly 1.8x Standard for higher rate limits and lower latency (2.5: $0.0702/image; Pro: $216 per 1M output tokens). **Not available on Nano Banana 2.**

So: if you need Flex/Priority consumption options, Pro is the only current-generation model that offers them. For everything else use Standard, and Batch for bulk.

---

## Search Grounding (Nano Banana 2 and Pro)

- **5,000 search requests per month free**, shared across all Gemini 3.x models
- **$14 / 1,000 requests** above that quota
- Nano Banana 2's Image Search Grounding is billed at the same rate as web search
- One user prompt may trigger more than one search query - you are billed per query, not per prompt
- **Lite and legacy 2.5 do not support grounding at all**

---

## What's NOT charged

- **"Thought images"** - Nano Banana 2 and Pro generate interim images while reasoning; visible in the backend, not billed.
- **SynthID watermarking** - automatic, no charge.
- **Retrieved grounding context** (text or images Search returns to the model) - not counted as input tokens. Only the search query is billed.

---

## Free playgrounds

- **Google AI Studio** (`aistudio.google.com`) - free interactive UI for all four models. Counts against per-account quotas, not free billing tokens. The right place to lock a prompt before spending API budget.
- **Gemini consumer app** - free, model unspecified. Not part of API quota.

---

## Rough cost guidance

| Scenario | Recommended model | Approx cost |
|---|---|---|
| 100 product photos for a catalogue | Lite (Batch) | ~$1.70 |
| 100 product photos, needs multi-reference consistency | Nano Banana 2 (Batch, 1K) | ~$3.40 |
| 50 YouTube thumbnails with bold text | Nano Banana 2 (Standard, 1K) | ~$3.35 |
| 50 thumbnails generated from the videos themselves | Nano Banana 2 (video input required) | ~$3.35 + video input tokens |
| 10 high-fidelity infographics with grounded data | Pro (Standard, 2K) | ~$1.34 + search queries |
| 1 hero image for a landing page (4K) | Pro (Standard, 4K) | $0.24 |
| Interactive edit loop in an app (5 turns) | Lite (Standard) | ~$0.17 |

---

## Source

- Official pricing page: https://ai.google.dev/gemini-api/docs/pricing (per-model sections and footnotes)
- Model cards: `/models/gemini-3.1-flash-image`, `/models/gemini-3.1-flash-lite-image`, `/models/gemini-3-pro-image`, `/models/gemini-2.5-flash-image`
- Deprecation dates: https://ai.google.dev/gemini-api/docs/deprecations
