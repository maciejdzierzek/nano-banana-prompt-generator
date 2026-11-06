# Nano Banana / Gemini Image Generation Pricing

**Verified against the official Gemini API pricing page** at [aistudio.google.com/docs/pricing](https://aistudio.google.com/docs/pricing) on **2026-11-06**. Pricing changes - verify in the Gemini API dashboard before quoting figures to anyone.

All prices are USD, paid tier. Image-generation models do **not** have a free tier - the Free Tier column shows "Not available" for all five.

---

## Per-image output cost (Standard tier)

| Model | 0.5K (512px) | 1K (1024px) | 2K (2048px) | 4K (4096px) |
|---|---|---|---|---|
| Nano Banana 2.1 (`gemini-nano-banana-2.1`) | - | **$0.0336** | $0.0504 | $0.113 |
| Nano Banana 2 Lite (`gemini-3.1-flash-lite-image`) | - | **$0.0336** | - | - |
| Nano Banana Pro (`gemini-3-pro-image`) | - | $0.134 | $0.134 | $0.24 |
| Nano Banana 2 (`gemini-3.1-flash-image`) ⚠️ | $0.045 | $0.067 | $0.101 | $0.151 |
| Nano Banana legacy (`gemini-2.5-flash-image`) ⚠️ | - | $0.039 | - | - |

⚠️ Nano Banana 2 is deprecated with no shutdown date, and Nano Banana 2.1 beats it on per-image price at every shared resolution. Legacy 2.5 shuts down 2027-03-15 and costs more than Lite for a worse result. There is no output-cost argument for staying on either.

**Token math (from the official pricing footnotes):**
- Nano Banana 2.1: image output $30 / 1M tokens. 1K = 1120 tok, 2K = 1680 tok, 4K = 3780 tok. (Note: the guide's aspect-ratio table lists 2520 tok for 4K, which would be $0.0756. The pricing page's 3780 tok is the figure behind Google's published $0.113, so that is the one used here.)
- Nano Banana 2 Lite: $30 / 1M tokens. 1K = 1120 tok → $0.0336.
- Nano Banana Pro: $120 / 1M tokens. 1K-2K = 1120 tok ($0.134), 4K = 2000 tok ($0.24).
- Nano Banana 2: $60 / 1M tokens. 0.5K = 747 tok, 1K = 1120 tok, 2K = 1680 tok, 4K = 2520 tok.
- Legacy 2.5: $30 / 1M tokens. 1024x1024 = 1290 tok ($0.039).

---

## Input and text cost (per 1M tokens, Standard tier)

| Model | Input | Text and thinking output |
|---|---|---|
| Nano Banana 2 Lite | $0.25 (text/image/video) | $1.50 |
| Legacy 2.5 | $0.30 (text/image) | same as Gemini 2.5 Flash |
| Nano Banana 2 | $0.50 (text/image) | $3.00 |
| Nano Banana 2.1 | **$1.50** (text/image/video) | **$7.50** |
| Nano Banana Pro | $2.00 (text/image), equivalent to **$0.0011 per input image** | $12.00 |

This is the catch in the Nano Banana 2.1 migration: image output is cheaper than on Nano Banana 2, but input is 3x and text/thinking 2.5x dearer, and 2.1 defaults to `medium` Thinking where Nano Banana 2 and Lite default to `minimal`. On a job heavy with reference images, video input or long prompts the per-image figure understates the bill. Read `usage` on a real call before extrapolating. The pricing page does not state how many tokens one input image costs on 2.1.

---

## Batch API (50% discount)

| Model | 1K | 2K | 4K |
|---|---|---|---|
| Nano Banana 2.1 | $0.0168 | $0.0252 | $0.0567 |
| Nano Banana 2 Lite | $0.0168 | - | - |
| Legacy 2.5 | $0.0195 | - | - |

Batch input and text rates: Nano Banana 2.1 $0.75 / $3.75, Lite $0.125 / $0.75, legacy 2.5 input $0.15.

**Nano Banana Pro and Nano Banana 2:** their model cards list Batch API as supported and the [Batch API page](https://aistudio.google.com/docs/batch-api) promises 50% of standard cost in general, but their pricing sections now show only a Standard tab. Half of Standard would be $0.067 / $0.12 on Pro. Treat that as unconfirmed until a real batch job shows it.

Use Batch for bulk generation where latency doesn't matter (e.g. 100 thumbnail variants overnight). At Batch prices Nano Banana 2.1 and Lite land at **under 2 cents an image** at 1K.

---

## Flex and Priority tiers

Every image model card marks Flex and Priority inference as **not supported**. The pricing page still carries Flex ($0.0195) and Priority ($0.0702) rates for legacy 2.5 only, which contradicts its card, and that model is on its way out. Do not plan around either tier for image work. Use Standard, and Batch for bulk.

---

## Search Grounding (Nano Banana 2.1, Nano Banana 2 and Pro)

- **5,000 search requests per month free**, shared across all Gemini 3 and newer models
- **$14 / 1,000 requests** above that quota
- Default cap of **1,500 requests per day** per project on the Gemini Developer API
- Image Search Grounding (Nano Banana 2.1 and Nano Banana 2) is billed at the same rate as web search
- One user prompt may trigger more than one search query - you are billed per query, not per prompt
- **Lite and legacy 2.5 do not support grounding at all**

---

## What's NOT charged

- **"Thought images"** - the Gemini 3 image models generate interim images while reasoning; visible in the backend, not billed. Thinking text tokens are billed at the text output rate.
- **SynthID watermarking** - automatic, no charge.
- **Retrieved grounding context** (text or images Search returns to the model) - not counted as input tokens. Only the search query is billed.

---

## Free playgrounds

- **Google AI Studio** (`aistudio.google.com`) - free interactive UI. Counts against per-account quotas, not free billing tokens. The right place to lock a prompt before spending API budget.
- **Gemini consumer app** - free, model unspecified. Not part of API quota.

---

## Rough cost guidance

Image output only, on Nano Banana 2.1 and Lite add input and thinking tokens.

| Scenario | Recommended model | Approx cost |
|---|---|---|
| 100 product photos for a catalogue | Nano Banana 2.1 or Lite (Batch, 1K) | ~$1.68 |
| 100 product photos, needs multi-reference consistency | Nano Banana 2.1 (Batch, 1K) | ~$1.68 + reference input tokens |
| 50 YouTube thumbnails with bold text | Nano Banana 2.1 (Standard, 1K) | ~$1.68 |
| 50 thumbnails generated from the videos themselves | Nano Banana 2.1 (Standard, 1K) | ~$1.68 + video input tokens |
| 10 high-fidelity infographics with grounded data | Pro (Standard, 2K) | ~$1.34 + search queries |
| 1 hero image for a landing page (4K) | Nano Banana 2.1, or Pro for dense text | $0.113, or $0.24 on Pro |
| 1 wide web banner, `8:1` at 2K | Nano Banana 2.1 (Standard) | $0.0504 |
| Interactive edit loop in an app (5 turns) | Lite (Standard) | ~$0.17 |

---

## Source

- Official pricing page: https://aistudio.google.com/docs/pricing (per-model sections and footnotes)
- Model cards: `/docs/models/gemini-nano-banana-2.1`, `/docs/models/gemini-3.1-flash-image`, `/docs/models/gemini-3.1-flash-lite-image`, `/docs/models/gemini-3-pro-image`, `/docs/models/gemini-2.5-flash-image`
- Deprecation dates: https://aistudio.google.com/docs/deprecations
