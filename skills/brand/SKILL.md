---
name: elevenlabs-brand
description: "Enforce ElevenLabs brand guidelines when creating any content — presentations, documents, HTML pages, social graphics, emails, landing pages, or marketing materials for ElevenLabs. Use this skill whenever creating or reviewing content that represents ElevenLabs, including slide decks, one-pagers, partner materials, certification pages, university content, blog posts, or any asset that carries the ElevenLabs name. Also trigger when the user mentions 'on-brand', 'brand check', 'ElevenLabs style', 'brand guidelines', or asks to make something look like ElevenLabs. Even if the user just says 'make a deck for ElevenLabs' or 'create an ElevenLabs landing page', use this skill to ensure brand compliance."
---

# ElevenLabs Brand Guidelines

This skill contains the official ElevenLabs brand rules extracted from the V1 Brand Guidelines (2025). Apply these rules whenever creating or reviewing content that represents ElevenLabs.

## Brand Name — Getting It Right

The company name is **ElevenLabs** — one word, capital E, capital L. This is the single most common mistake people make, so pay close attention.

**Correct:** ElevenLabs

**Incorrect — all of these are wrong:**
- "Eleven Labs" (no space between the words)
- "Elevenlabs" (the L must be capitalized)
- "elevenlabs" (both E and L must be capitalized)
- "ELEVENLABS" (not all caps)
- "IIElevenLabs" (don't use capital I's to recreate the 11 icon in text)
- "11ElevenLabs" (don't use the number 11 in text to recreate the icon)

The "II" symbol (two vertical bars) is only part of the **logo/wordmark** — it is the visual icon, not part of the written name. When writing the company name in running text, emails, documents, slide copy, etc., just write "ElevenLabs" without the II prefix.

## Logotype Rules

The logo is a custom wordmark with the II symbol (two vertical lines representing "11") followed by "ElevenLabs" as one word. It has a powerfully simple design.

**Safespace:** The logo needs ample, unobstructed space around it. The minimum clearance area equals the height of the logo on all sides.

**The II Symbol:** The visual shorthand for the brand — two distinctively simple vertical lines. It abstracts the number eleven while subtly referencing a pause icon (connecting to audio). Available in three sizes: Large (80px), Medium (40px), Small (24px).

**What to avoid with the logotype:**
- Don't stack the wordmark or alter the 11 icon
- Don't use a shortened version (like "IILabs")
- Don't apply effects like drop shadows
- Don't apply strokes or outlines
- Don't rotate or angle the logo
- Don't apply color to the logo (it's always black or white)
- Don't stretch or deform the logo
- Don't recreate the logo in other fonts

## Typography

**Brand font: KMR Waldenburg** — used exclusively, in two weights:
- **KMR Waldenburg Book** — the lighter weight, used for refined/light headlines and body text
- **KMR Waldenburg Normal** — the heavier weight, used for bold/confident headlines and emphasis

The typeface blends humanist and organic elements with low stroke contrast and prominent vertical lines.

**Typographic scale (from brand guidelines PDF):**

| Use | Weight | Size | Line-Height | Tracking |
|-----|--------|------|-------------|----------|
| Hero headline | Book | 80px | 100% | 0% |
| Large headline | Normal | 58px | 100% | -2% |
| Body / description | Book | 27px | 150% | 0% |
| Body small | Book | 18px | 140% | 0% |

**Only two weights are used in the brand system:** Book (light/refined) and Normal (bold/confident). The font ships with Halbfett and Fett weights as well, but these are not part of the official typographic hierarchy.

The key principle is **typographic balance** — weight, size, line height, and tracking combine to create rhythm, contrast, and harmony. Lighter weight (Book) for large headlines gives a refined look; heavier weight (Normal) for secondary headlines adds confident emphasis.

**When KMR Waldenburg is unavailable** (which it often will be when working in code, web, or standard office tools), use a clean sans-serif substitute that shares similar characteristics: low stroke contrast, geometric-humanist feel. Good fallbacks include Inter, Helvetica Neue, or similar.

## Color Palette

The palette is **minimal and monochrome** at its foundation, with color coming from imagery and media rather than applied UI colors.

**Core Colors:**

| Name | Hex | RGB | Use |
|------|-----|-----|-----|
| Black | #000000 | R0 G0 B0 | Primary text, dark backgrounds |
| Graphite | #1E1916 | R30 G25 B22 | Warm dark alternative |
| Off-White | #FAFAFA | R250 G250 B250 | Light backgrounds |
| Cream | #F5F3F1 | R245 G242 B241 | Warm light backgrounds |
| Light-Gray | #E0DEDC | R224 G222 B220 | Subtle dividers, secondary bg |
| Image-Based Accent | — | — | Color comes from imagery, not a fixed swatch |

**Color proportion principle:** The overall layout should remain mostly clean and monochrome, with color accents making up a smaller portion of the visual space. Color comes from imagery and backgrounds — it emerges from visual compositions, not from applied UI palettes. Gray shades should not be used with color accents.

**Two color modes by content type:**

| Mode | Colors | Applications | Examples |
|------|--------|-------------|----------|
| Core Brand | Black, white, off-white + image-based accents | General / marketing | New features, product releases, partnerships |
| Monochrome | Black, white, grays only | Technical / developer | Documentation, case studies, product guides |

Developer content like documentation or guides uses only black, white, and gray — no colorful imagery. Third-party photos, logos, or assets may be included when relevant.

## Imagery

Images serve as the primary source of color in the brand. They should feel **saturated, dreamlike, and imaginative**, pointing to a hopeful, sensory future.

**Image treatment rules:**
- Use royalty-free or AI-generated images with a bright, saturated palette
- Motion blur or movement helps convey a sound atmosphere
- Images are treated with grain and blur so they dissolve into tone, light, and texture — emphasizing atmosphere rather than literal content
- Even if not fully visible, imagery should follow a "sound metaphor" — evoking what you might hear

**Noise texture — required on ALL assets:**
- Apply a subtle noise overlay to every asset
- Specific values: **Tile at 30%, Overlay blend mode, 70% opacity**
- Must be delicate — should not alter saturation, luminosity, or quality

**Simple blur treatment:**
- Apply a layer blur (value ~25) to soften shapes and create warmth/atmosphere

**Complex blur treatment (signature technique):**
- Use TWO blur layers instead of blurring the base image directly
- First layer: heavy blur over the entire composition
- Second layer: lighter blur with a Chladni shape mask, revealing parts of the image below
- The base image itself remains unedited
- This creates the distinctive "clarity through shape" effect seen throughout the brand

**Blur framing rules:**
- Avoid tight crops that turn images into formless color patches — keep some structure
- Don't mix too many colors in one crop (prevents visual noise)
- Don't over-blur to the point colors become muddy
- With complex blur, keep composition clean and ensure text/logos remain legible

**AI image generation guidance:**
- Use reference images to define palette or setting
- Prompts should capture atmosphere, with camera styles or framing added if needed
- Keep images evocative and atmospheric
- Use moodboards or reference links for consistency across images

## Graphics — Chladni Patterns & Shapes

ElevenLabs uses two distinct graphic elements derived from sound visualization. Both connect to the mission of making sound visible, but they look and function differently.

### Chladni Patterns (grid-based interference textures)

These are mathematically generated from the Chladni equation: `D = a sin(mx) sin(ny) + b sin(nx) sin(my)`. They produce **fine grids of dots and lines** forming complex geometric interference patterns — NOT concentric circles or radial lines. They look like intricate woven or lattice structures.

**Usage:**
- Can appear monochrome or on colored backgrounds
- May be layered over imagery and combined with subtle noise textures
- Chladni patterns in monochrome are mostly used for developer-related content
- Opacity and stroke weight should be adjusted to keep them subtle and sophisticated yet visible

### Chladni Shapes (organic curvilinear mask forms)

These are organic, blob-like shapes with smooth curves — also derived from sound mathematics but used differently from the grid patterns. They function as **masks over photography** to create distinctive compositions.

**Signature treatment — "Clarity and Focus":** Sharp Chladni shapes reveal clear imagery through a blurred surrounding, emphasizing precision and clarity. This is the brand's most distinctive visual technique.

**Usage:**
- Used as clipping masks over blurred imagery
- The shape is sharp while the surrounding image is blurred
- Creates a "window of clarity" effect
- Often combined with the complex blur technique (two blur layers + shape mask)

## Graphics — Linear Illustrations

Linear illustrations bring structure and highlight ElevenLabs' research-driven perspective. They can be applied in monochrome and in combination with color, adapting flexibly across assets. Examples include concentric circles, isometric cubes, and geometric line drawings.

## Layout Composition Principles

Based on the application examples throughout the guidelines, these patterns emerge:

**Standard layout formula:**
1. Logo/wordmark in the top-left corner (small)
2. Large headline text in the lower-left area
3. Imagery or graphic element on the right side or as background
4. Plenty of whitespace — the brand breathes

**Split compositions:** Many layouts use a clean half (text, logo, white/light background) paired with a visual half (blurred imagery, Chladni patterns, color). This creates a strong contrast between clarity and atmosphere.

**Dark vs. light:** Both work. Dark backgrounds use white text and white logo. Light backgrounds use black text and black logo. The brand works confidently in both directions.

## Quick Reference Checklist

When creating any ElevenLabs-branded content, verify:

1. **Name spelling** — "ElevenLabs" (one word, capital E and L, no II prefix in text)
2. **Color palette** — Monochrome foundation; color from imagery only
3. **Typography** — KMR Waldenburg (or clean sans-serif fallback); proper weight hierarchy
4. **Logo usage** — Sufficient safespace, no effects, no color, no distortion
5. **Imagery** — Saturated, dreamlike, atmospheric; blur + grain treatment
6. **Layout** — Clean, spacious, monochrome base with imagery accents
7. **Content type** — Core Brand (with color imagery) vs. Monochrome (developer/technical)
8. **Graphics** — Chladni patterns and shapes where appropriate
9. **Noise texture** — Subtle overlay on assets
10. **Overall feel** — Minimal, confident, warm but precise
