# ElevenLabs Brand Kit — Agent Guide

This file provides brand guidelines and asset usage instructions for any AI agent working with ElevenLabs brand materials. Follow these rules to ensure all output is on-brand.

**Source:** [github.com/jakeat11labs/elevenlabs-brand-kit](https://github.com/jakeat11labs/elevenlabs-brand-kit)

---

## Brand Identity

ElevenLabs is a voice AI company. The visual identity is **minimal, monochrome, and precise** — color emerges from atmospheric imagery rather than applied palettes. The aesthetic conveys warmth, precision, and a connection to sound.

### The Name

**ElevenLabs** — one word, capital E, capital L.

Never write it as:
- "Eleven Labs" (no space)
- "elevenlabs" or "Elevenlabs" (capitalization matters)
- "ELEVENLABS" (not all caps)
- "IIElevenLabs" or "11ElevenLabs" (the II symbol is logo-only, never in text)

### Logo Rules

- Always black OR white — never colored
- Minimum clearance = logo height on all sides
- Never rotate, shadow, outline, distort, stretch, or stack
- The "II" symbol (two vertical bars) is the brand icon, referencing "11" and a pause button
- Never recreate the II icon using text characters

---

## Two Visual Modes

Every piece of ElevenLabs content uses one of two modes:

### Core Brand Mode (default for most content)

- Full-color gradient backgrounds from the brand image library
- White text on dark/gradient backgrounds
- Noise texture overlay (70% opacity, overlay blend)
- Blur treatment on imagery
- **Use for:** marketing, educational content, product launches, training, academy content, onboarding, partnerships, internal communications

### Monochrome Mode (developer content only)

- Black, white, and gray only — no brand imagery or color accents
- **Use for:** API documentation, SDK references, technical specs, changelogs
- Third-party logos/images are acceptable when relevant

**Key distinction:** If the content teaches, inspires, or onboards users, it is Core Brand — not Monochrome. Monochrome is strictly for developer reference material.

---

## Colors

| Token | Hex | Purpose |
|-------|-----|---------|
| Graphite | #1E1916 | Primary text color. Never use as a background fill. |
| Off-White | #FAFAFA | Page/scene backgrounds (Content mode) |
| Cream | #F5F3F1 | Card backgrounds, accent areas |
| Black | #000000 | Dark backgrounds, primary text alternative |
| Light-Gray | #E0DEDC | Borders, dividers, subtle separators |
| Mid-Gray | #999999 | Secondary text, labels |
| Near-Black | #151515 | Icon fill on cream cards, dark mode text |
| Amber | #F59E0B | Annotations, callout markers, highlights |
| White | #FFFFFF | Text on dark/gradient backgrounds (Hero mode) |

**Color principle:** The layout stays monochrome; imagery provides the color. No gray shades with color accents.

---

## Typography

**Font:** KMR Waldenburg (exclusively)
- **Book** (weight 300) — light, refined. Hero headlines and body text.
- **Normal** (weight 400) — bold, confident. Emphasis headlines.

**Fallback when KMR is unavailable:** Inter, Helvetica Neue, or similar clean sans-serif.

| Use | Weight | Size | Line-Height | Tracking |
|-----|--------|------|-------------|----------|
| Hero headline | Book | 80px | 100% | 0% |
| Large headline | Normal | 58px | 100% | -2% |
| Body / description | Book | 27px | 150% | 0% |
| Body small | Book | 18px | 140% | 0% |

**Only two weights** are used in the brand system: Book and Normal. Halbfett and Fett exist in the font files but are not part of the official typographic hierarchy.

---

## Imagery Treatment

All brand imagery must be treated with:

1. **Noise overlay** — 70% opacity, overlay blend mode. Always present, always subtle. Must not alter saturation, luminosity, or quality.
2. **Blur** — Simple blur (~25) for atmosphere, or complex blur (two layers: heavy 180 blur + reveal layer at 0.1 opacity through a Chladni shape mask). The base image stays unedited.

**Image qualities:** Saturated, dreamlike, atmospheric, evoking sound. Use royalty-free or AI-generated images with bright, saturated palettes. Motion blur conveys sound atmosphere.

**Blur rules:**
- Avoid tight crops that turn images into formless color patches — keep structure
- Don't mix too many colors in one crop
- Don't over-blur to muddy colors
- Ensure text and logos remain legible over blurred imagery

---

## Chladni Patterns & Shapes

Core brand elements from sound visualization:

- **Chladni Patterns** — mathematically generated grid interference textures (fine dots and lines forming woven/lattice structures). Used as overlays. Monochrome patterns are for developer content.
- **Chladni Shapes** — organic, blob-like curvilinear masks over photography. Creates the signature "clarity through shape" effect — sharp shapes reveal clear imagery through a blurred surrounding.

---

## Asset Directory

```
brand-assets/
├── backgrounds/
│   ├── gradient/           # 16 atmospheric gradients (Background0-16.jpg)
│   ├── agents/             # 9 green/teal/blue agent-branded backgrounds
│   ├── chladni-closeup/    # 15 landscape/nature base layers
│   ├── general/            # 4 gradient + 3 Chladni pattern backgrounds
│   ├── creative/           # 1 coral-sunset warm gradient
│   ├── noise-texture.jpg   # Noise overlay (mix-blend-mode: overlay, 80% opacity)
│   └── seamless-texture.jpg # Seamless texture (mix-blend-mode: soft-light)
├── icons/
│   ├── cream/              # 130 icons on #F5F3F1 cream background
│   ├── black/              # 130 icons in black on transparent
│   ├── white/              # 130 icons in white on transparent
│   └── product/            # 10 product-specific icons
├── voice-orbs/             # 8 metallic spheres on black backgrounds
├── logos/                  # Black + white variants, PNG + SVG
├── fonts/                  # KMR Waldenburg OTFs
├── diagrams/               # Architecture diagrams
├── ui-highlights/          # UI screenshot references
├── covers/                 # Chladni pattern cover images
├── color-tokens.json       # Full color token definitions
├── BRAND_GUIDELINES.md     # Official 2025 guidelines (59 slides)
└── index.html              # Visual catalog — browse all assets in browser
```

---

## Icon Selection

| Context | Use This Variant |
|---------|-----------------|
| Dark/gradient background (Hero mode) | `icons/white/` — always |
| Light background (Content mode) | `icons/black/` |
| Dark cards or dark compositions | `icons/cream/` |
| Product callout on dark background | `icons/product/` + CSS `filter: brightness(0) invert(1)` |

Icon naming format: `{idx:03d}-{clean-name}.png` (e.g., `015-zap-lightning-flash-thunder.png`). Open `icons/catalog.html` in a browser to search and preview.

---

## Voice Orbs

8 metallic spheres (teal, purple, pink, coral, green, gold, hotpink, lavender) — all 1174x1038px JPGs on black backgrounds. To composite:

- **Web/CSS:** `mix-blend-mode: screen` knocks out the black
- **Remotion:** `style={{ mixBlendMode: 'screen' }}`
- Use as animated accent elements: scale, rotate, pulse, parallax float

---

## Layout Principles

- Logo/wordmark in top-left corner (small)
- Large headline in lower-left
- Imagery or graphic element on right side or as background
- Plenty of whitespace — the brand breathes
- Split compositions work well: clean half (text, logo, light bg) paired with visual half (blurred imagery, Chladni patterns, color)
- Both dark and light backgrounds work — dark uses white text/logo, light uses black text/logo

---

## Using Assets on the Web

```html
<img src="/brand-assets/backgrounds/gradient/Background0.jpg" alt="" />
<img src="/brand-assets/icons/white/015-zap-lightning-flash-thunder.png" alt="" />
<img src="/brand-assets/voice-orbs/orb-teal-on-black.jpg" alt="" style="mix-blend-mode: screen;" />
```

```css
background-image: url('/brand-assets/backgrounds/gradient/Background0.jpg');
```

## Using Assets in Remotion

```tsx
import { Img, staticFile } from 'remotion';

<Img src={staticFile('brand-assets/backgrounds/gradient/Background0.jpg')} />
<Img src={staticFile('brand-assets/icons/white/015-zap-lightning-flash-thunder.png')} />
<Img src={staticFile('brand-assets/voice-orbs/orb-teal-on-black.jpg')}
     style={{ mixBlendMode: 'screen' }} />
```

---

## Compliance Checklist

Before delivering any ElevenLabs-branded content, verify:

- [ ] Name spelled "ElevenLabs" (one word, capital E and L)
- [ ] Using KMR Waldenburg font (or approved sans-serif fallback)
- [ ] Correct visual mode (Core Brand for most content; Monochrome only for dev API docs)
- [ ] Colors from the approved palette only — no unauthorized accent colors
- [ ] Color comes from imagery, not applied UI palettes
- [ ] Logo has proper safespace, is black or white only, unmodified
- [ ] Imagery treated with noise overlay (70% opacity, overlay blend) and blur
- [ ] Icon variant matches background (white on dark, black on light)
- [ ] Layout is clean and spacious — the brand breathes
- [ ] No gray shades mixed with color accents
- [ ] Overall feel: minimal, confident, warm, precise
