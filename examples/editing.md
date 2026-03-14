# Photo Editing Examples

Ready-to-use editing prompts. Always include a CRITICAL: preserve section when elements must remain unchanged.

---

## Object Removal

**Use case:** Clean up backgrounds, remove distracting elements, prepare product shots.

```
Remove the power cable visible in the lower right corner of this photo.
Fill the space with floor texture that matches the surrounding area.
Preserve lighting consistency and natural shadows.
Seamless integration, no visible editing artifacts.

CRITICAL: Preserve the product, person, and all other background elements exactly as they are.
```

**Expected result:** The specified object disappears with plausible background fill. "CRITICAL: preserve" tells the model what must stay untouched - always include this to avoid unintended changes.

---

## Style / Season Change

**Use case:** Real estate visuals, landscape transformations, mood adjustments.

```
Transform this outdoor scene into early winter.
Keep the building facade, windows, and street layout exactly the same.
Add light snow on rooftops and window ledges, change deciduous trees to bare branches.
Overcast flat light, desaturated color palette with cool blue-grey tones.
Photorealistic result, natural color grading.

CRITICAL: Preserve the building architecture, all signage text, and street furniture exactly as shown.
```

**Expected result:** Seasonal transformation feels natural. Structural elements stay accurate. The explicit CRITICAL section prevents the model from redesigning the building or removing signs.

---

## Text Localization

**Use case:** Translating marketing materials, product labels, UI screenshots for international use.

```
Translate all English text in this image to Polish.
Keep everything else identical: design, colors, layout, imagery, proportions.
For each text element: match the original font weight (bold/regular), size, and color.
Do not rearrange any elements - only replace the text content.
Final image must be indistinguishable in design from the original, only language changes.

CRITICAL: Preserve all visual design, icons, background imagery, and non-text elements exactly unchanged.
```

**Expected result:** Text is translated while the overall design stays intact. Works well for infographics, product labels, social media templates. Quality varies by language complexity.

---

## Thumbnail Enhancement

**Use case:** Improving existing YouTube thumbnails for higher CTR without recreating them.

```
Enhance this YouTube thumbnail for maximum click-through rate.

ENHANCE:
- Increase sharpness by 40%
- Boost color saturation by 35%
- Increase contrast by 30%
- Add subtle rim lighting on key subjects

CRITICAL: Preserve exactly - person's face, expression, and pose unchanged. Product shape and design identical. All text elements, positions, and content unchanged. Overall composition and layout unchanged.

TECHNICAL: 1280x720px, 16:9, YouTube-optimized
```

**Expected result:** Punchier, more eye-catching version of the original thumbnail. Face and text stay intact. The contrast between ENHANCE and CRITICAL sections gives the model clear boundaries for what to change vs. protect.

> **Key principle:** Every editing prompt should have an ENHANCE/CHANGE section and a CRITICAL: PRESERVE section. This structure consistently produces better results than describing only what to change.
