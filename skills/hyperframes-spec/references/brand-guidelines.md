# ElevenLabs Brand Guidelines 2025 — Video Application

> Based on the official Brand Design System (External) from Figma file `0AzA12gk3NEAqRLwRKnAPc`.
> Updated for the v2 two-mode scene system.

---

## Two-Mode Scene System

Every Remotion scene operates in one of two visual modes. The mode determines background treatment, text color, card style, and watermark variant.

### Mode 1: Hero (Gradient Backgrounds)
- Full-bleed background image with blur + noise + optional Chladni overlay
- WHITE text (#FFFFFF) for all headlines and body text
- Frosted cream cards: `rgba(245, 243, 241, 0.88)` with `backdrop-filter: blur(24px)`, border `1px solid rgba(255,255,255,0.12)`, radius 14px, shadow `0 8px 32px rgba(0,0,0,0.12)`
- WHITE II watermark at 25% opacity
- **Use for:** Title cards, module openers, concept reveals, methodology scenes, outros, atmospheric moments

### Mode 2: Content (Light Backgrounds)
- OFF_WHITE (#FAFAFA) solid background
- GRAPHITE (#1E1916) text for headlines, DARK_GRAY (#555555) for body
- Solid CREAM (#F5F3F1) cards with LIGHT_GRAY (#E0DEDC) borders
- GRAPHITE II watermark at 35% opacity
- **Use for:** Step sequences, code examples, detail content, numbered lists, checklists, comparisons

---

## Brand Name

**ElevenLabs** — one word, capital E, capital L. Never "Eleven Labs", "elevenlabs", "ELEVENLABS", "IIElevenLabs", or "11ElevenLabs". The "II" symbol is the visual icon only, not part of the written name.

---

## Logotype

Custom wordmark: II symbol (two vertical lines = "11") + "ElevenLabs" as one word. Described as: **Modular, Sharp & Precise**.

**Safespace:** Minimum clearance = height of the logo on all sides.

**II Symbol:** "The visual shorthand for our brand is our 11 icon, constructed from two distinctively simple vertical lines. It abstracts the number eleven from our name, while also subtly referencing the pause icon seen when listening to audio."

**II Symbol sizes:** Large (80px height), Medium (40px), Small (24px).

**Never:** stack the wordmark, alter the 11 icon, use shortened versions, apply effects/shadows/strokes/outlines, rotate, apply color (always black or white), stretch/deform, recreate in other fonts.

---

## Typography

### Brand Font: KMR Waldenburg

"Typography is set exclusively in KMR Waldenburg, used in two weights to create a clear and cohesive visual hierarchy."

**Two weights only:**
- **Normal (400)** — bold, confident. For emphasis, medium headlines.
- **Book (300)** — refined, light. For large headlines, body text, subheads.

"The typeface blends humanist and organic elements. Its precise construction is characterized by low stroke contrast and prominent vertical lines."

### Typographic Scale

| Use | Weight | Size | Line-Height | Tracking |
|-----|--------|------|-------------|----------|
| Hero headline | Book (300) | 80px | 100% | -3% |
| Large headline | Normal (400) | 58px | 100% | -2% |
| Subhead / intro | Book (300) | 27px | 120% | -1% |
| Body text | Book (300) | 18px | 140% | 0% |
| Labels | Semibold (600) | 14px | -- | 8% uppercase |

### Typographic Balance
"It's not only about hierarchy but how text elements relate. Weight, size, line height, and tracking combine to create rhythm, contrast, and harmony across formats." Lighter weight for large headlines (refined), heavier weight for emphasis.

### Video-Specific Sizes
- Scene titles: 44-68px
- Card names: 28px
- Descriptions: 16-17px
- Labels: 14px

---

## Color Palette

### Primary Colors

"The primary palette is minimal and monochrome, forming a clean foundation. Color comes mainly from images or video."

| Name | Hex | Use |
|------|-----|-----|
| **Graphite** | #1E1916 | Primary text (Content mode), warm dark |
| **Off-White** | #FAFAFA | Content mode backgrounds |
| **Cream** | #F5F3F1 | Card fills, warm light surfaces |
| **Light-Gray** | #E0DEDC | Dividers, borders, secondary backgrounds |
| **Mid-Gray** | #999999 | Labels, secondary text |
| **Dark-Gray** | #555555 | Body text (secondary) |
| **White** | #FFFFFF | Text on Hero scenes, Hero watermarks |
| **Black** | #000000 | Pure black (use sparingly) |

### Color Rules

1. **Color is image-based** — it comes from background imagery, not from separate color palettes. Hero scenes derive their color from the background image.
2. **No gray with color accents** — "Gray shades should not be used with color accents." When a scene has colorful imagery (Hero mode), cards and text stay white/cream. No MID_GRAY or DARK_GRAY mixed with the image color.
3. **Color proportion** — "The overall layout should remain mostly clean and monochrome, with color accents making up a smaller portion of the visual space."

### Color Application Modes
1. **Core Brand** (Hero mode in our system) — "New features, product releases, partnerships." Full brand palette with imagery, blur, noise.
2. **Monochrome** (Content mode in our system) — "Developers, case studies, product insights, guides." Black, white, and gray only. No brand imagery.

---

## Image Use

### Philosophy
"Images bring in abstract color, treated with grain and blur so they dissolve into tone, light and texture, emphasizing atmosphere rather than literal content."

"Even if not fully visible, imagery should follow a sound metaphor — through masks or patterns, it should still evoke a sound environment, helping users see what they might otherwise hear."

### Image Selection
- "Images should feel saturated, dreamlike and imaginative, pointing to a hopeful, sensory future."
- "Motion blur or movement helps convey the desired sound atmosphere."
- Use royalty-free or AI-generated images with a bright, saturated palette.

---

## Noise Treatment

**Apply noise to EVERYTHING** — both Hero and Content mode scenes.

- **Opacity:** 70%
- **Blend mode:** Overlay
- "Apply a subtle noise effect to all assets. Ensure it is delicate and does not alter saturation, luminosity or quality."

---

## Blur Treatment (Hero Mode)

Background images on Hero scenes must be blurred. Two options:

### Simple Blur
- **Value:** 25
- "This softens shapes and creates warmth and atmosphere."
- Use for most Hero scenes.

### Complex Blur
- **Layer 1:** Heavy blur (180) over the full composition
- **Layer 2:** Lighter blur with opacity 0.1, revealing parts of the base image below
- The base image remains unedited.
- Use for scenes where you want more visual depth — partial image reveals through the blur.

### Blur Rules
- "Avoid tight crops that turn images into color patches — keep some structure."
- "Do not mix too many colors in one crop to prevent visual noise."
- "Do not over-blur to muddy colors."
- "With complex blur, keep composition clean and ensure text or logos remain legible."

---

## Chladni Patterns

"Chladni patterns emerge when sound vibrations form visible shapes, turning sound into something we can see. They highlight our deep connection to voice and audio and are a core element of ElevenLabs' visual language."

### Usage
- Can appear in monochrome or on colored backgrounds
- Can be layered over imagery and combined with noise textures
- Can serve as masks over images for distinctive compositions
- "Sharp Chladni shapes over blurred imagery" creates contrast that "emphasizes focus, clarity, and the precision of our technology."

### Available Chladni Assets
- **15 chladni-closeup backgrounds** in `brand-assets/backgrounds/chladni-closeup/` — pre-blurred, ready to use as Hero backgrounds
- **3 Chladni pattern tiles** in `brand-assets/backgrounds/general/` (image38, image42, image50)
- **Chladni covers** bundled with this skill in `assets/covers/` (Cover0-3.jpg)
- **SVG ChladniPattern component** — code-generated for animated overlays (see `references/chladni-patterns.md`)

### Monochrome Chladni
- "Mostly used for developer-related content."
- "Opacity and stroke weight should be adjusted to keep them subtle and sophisticated yet visible."
- For Content mode: MID_GRAY or LIGHT_GRAY color, opacity 0.03-0.06

---

## Layout Composition

- Logo/wordmark top-left (small)
- Large headline text lower-left
- Imagery/graphics on right or as background
- Generous whitespace and padding (80px+ on sides)
- Split compositions: clean half + visual half
- Left-aligned text flow

---

## Video-Specific Guidelines

- **Scene alternation:** Alternate Hero and Content scenes for visual rhythm — atmospheric moments interspersed with clear instructional content.
- **Title scenes (Hero):** II logo + label + large title + breadcrumb, over gradient background
- **Outro scenes (Hero):** Centered II logo + summary + "up next", over gradient background
- **Content scenes:** Section title + subtitle + animated content, on OFF_WHITE
- **Transitions:** Fade only, 20 frames, between every scene
- **All animation via `useCurrentFrame()`** — never CSS transitions or Tailwind animate classes
- **II watermark on every scene** — WHITE variant on Hero, GRAPHITE variant on Content

---

## Key Brand Rules Summary

1. **Font:** KMR Waldenburg only. Two weights: Normal (400) and Book (300).
2. **Colors:** Monochrome foundation. Color comes from images, not palettes.
3. **No gray with color accents.**
4. **Noise on EVERYTHING:** 70% opacity, overlay blend mode.
5. **Blur:** Simple blur (25) or Complex blur (180 + 0.1 reveal layer). Base image stays unedited.
6. **Two modes:** Hero (gradient bg, white text, frosted cards) and Content (OFF_WHITE bg, graphite text, solid cream cards).
7. **Chladni patterns:** Core brand element. Used as overlays, masks, backgrounds, and standalone graphics.
8. **Logo:** Never alter, never color, never rotate. Safespace = logo height.
9. **Wordmark:** Always "ElevenLabs" — one word, capital E and L.
10. **Images:** Saturated, dreamlike, atmospheric. Sound metaphor. Always treated with noise + blur.
