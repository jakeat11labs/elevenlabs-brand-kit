# ElevenLabs Brand System for Remotion

## Color Palette (Monochrome Mode)

All ElevenLabs University content uses monochrome mode. No color. Ever.

```typescript
// src/brand.ts — import as: import { BRAND } from "../brand";
export const BRAND = {
  BLACK: "#000000",
  GRAPHITE: "#1E1916",     // Text and graphic elements. NEVER backgrounds.
  OFF_WHITE: "#FAFAFA",    // Every scene background. The canvas.
  CREAM: "#F5F3F1",        // Card backgrounds, panel fills.
  LIGHT_GRAY: "#E0DEDC",   // Dividers, borders, decorative numbers.
  MID_GRAY: "#999999",     // Labels, secondary text, section labels.
  DARK_GRAY: "#555555",    // Body text, descriptions.
  WHITE: "#FFFFFF",

  FONT_FAMILY: "'KMR Waldenburg', 'Inter', 'Helvetica Neue', sans-serif",
  HERO_SIZE: 80,       // Hero headlines — weight 300, tracking -3%
  HEADLINE_SIZE: 58,   // Section headlines — weight 300, tracking -2%
  SUBHEAD_SIZE: 27,    // Subheads — weight 400, tracking -1%
  BODY_SIZE: 22,       // Body text — weight 400
  LABEL_SIZE: 14,      // Labels — weight 600, uppercase, tracking 8%
} as const;
```

## Typography Scale

| Use | Size | Weight | Tracking | Color |
|-----|------|--------|----------|-------|
| Hero headline | 80px | 300 (Book) | -3% | GRAPHITE |
| Scene title | 44-68px | 300 (Book) | -2% to -3% | GRAPHITE |
| Section label | 14px | 600 (Semibold) | +10%, uppercase | MID_GRAY |
| Card product name | 28px | 400 (Normal) | -1% | GRAPHITE |
| Card tagline | 15px | 600, uppercase | +4% | MID_GRAY |
| Card description | 16-17px | 300 | 0 | DARK_GRAY |
| Subtitle / module info | 18px | 300 | 0 | DARK_GRAY |
| Hero words (LEARN/BUILD/PROVE) | 80px | 400 | +2% | GRAPHITE |
| Decorative numbers (01, 02...) | 120px | 300 | 0 | LIGHT_GRAY |
| Bottom captions | 15px | 300 | +2% | MID_GRAY |

**Weight principle:** Light weight (300) for large text. Heavier (400-600) for small text and labels.

## Font Loading

Fonts are loaded in `src/brand.ts` using `@remotion/fonts`:

```typescript
import { loadFont } from "@remotion/fonts";
import { staticFile } from "remotion";

const fontBook = loadFont({
  family: "KMR Waldenburg",
  url: staticFile("fonts/KMR-Waldenburg-Buch.otf"),
  format: "opentype",  // REQUIRED for .otf files
  weight: "300",
});

// Also: Normal (400), Halbfett (600), Fett (700)

export const waitForFonts = () =>
  Promise.all([fontBook, fontNormal, fontSemibold, fontBold]);
```

**The `format: "opentype"` is required.** Auto-detection doesn't work for .otf files.

## Spacing

- **Standard content padding:** `80px 120px` (containerStyle in brand.ts)
- **Card grid padding:** `60px 100px`
- **Card internal padding:** `40px 36px`
- **Card gap:** 28px
- **Card border radius:** 16px
- **Split layouts:** Left 52%, Right 48% (title card) or 35%/65% (split list)

## Card Styling

Every card in the system follows this pattern:

```tsx
{
  backgroundColor: BRAND.CREAM,
  border: `1px solid ${BRAND.LIGHT_GRAY}`,  // or 2px GRAPHITE for focus card
  borderRadius: 16,
  padding: "40px 36px",
}
```

Focus cards (the "Cert Focus" item):
- Border: `2px solid ${BRAND.GRAPHITE}`
- Badge: GRAPHITE background, OFF_WHITE text, 11px, weight 600, uppercase, tracking 8%, padding 5px 12px, radius 6px

## Persistent Elements

### NoiseOverlay
On EVERY scene at opacity 0.04:
```tsx
<NoiseOverlay opacity={0.04} />
```

### IIWatermark
On EVERY scene EXCEPT the title card (Scene 1):
```tsx
<IIWatermark />  // Bottom 32px, right 32px, opacity 0.35
```

### ChladniPattern
Subtle SVG background texture. Three variants: `organic`, `geometric`, `dense`.
- Content scenes: opacity 0.03-0.04
- Title/outro scenes: opacity 0.10-0.12
- Card accents: opacity 0.04 in corners

## Rules

1. **All backgrounds are OFF_WHITE** — no dark scenes
2. **GRAPHITE is for text/elements only** — never backgrounds
3. **"ElevenLabs"** — one word, capital E, capital L
4. **II watermark** on all scenes except the title card
5. **Noise overlay** on every scene at 0.04
6. **Generous padding** — content breathes, within 85% safe zone

## V2 Brand System (Hero Mode)

V2 introduces a two-mode system. V1 rules above still apply for Content mode scenes. Hero mode adds:

### GradientBackground
Full-bleed background image with blur + noise + tint. Component at `src/components/v2/GradientBackground.tsx`.
```tsx
<GradientBackground src="brand-assets/backgrounds/gradient/Background6.jpg" />
```
Props: `src` (staticFile path), `blur` (default 25), `noiseOpacity` (default 0.07), `tint` (default 0.12).

### BrandedCard (Frosted Cream)
Glassmorphism card for Hero scenes. Component at `src/components/v2/BrandedCard.tsx`.
```tsx
<BrandedCard style={{ padding: "48px 40px" }}>{children}</BrandedCard>
```
Styles: `rgba(245,243,241,0.88)` bg, `backdrop-filter: blur(24px)`, border `1px solid rgba(255,255,255,0.12)`, radius 14px, shadow `0 8px 32px rgba(0,0,0,0.12)`.

### Hero Mode Colors
| Element | Value |
|---------|-------|
| Headlines | WHITE (`#FFFFFF`) |
| Labels | `rgba(255,255,255,0.6)` |
| Dividers | `rgba(255,255,255,0.3)` |
| Accent lines | `rgba(255,255,255,0.6)` |
| Card text (titles) | `#151515` |
| Card text (descriptions) | `#666666` |
| Decorative numbers | `rgba(30,25,22,0.06)` |
| II Watermark | WHITE, use `<IIWatermark color="white" />` |

### Hybrid Mode (White/Gradient Split)
Used for PillarDetail and Timeline scenes. White left side (45%) with GRAPHITE text, gradient right side (55%) with frosted BrandedCards. IIWatermark uses `color="graphite"`.

### II Icon Opacity
- Title card, welcome card, outro: **full opacity** (no dimming)
- Never use `opacity: 0.4` or similar on the II icon in Hero scenes

### Background Assets
Backgrounds live in `public/brand-assets/backgrounds/`. Categories:
- `gradient/` — 18 atmospheric gradients (Background0-15.jpg, Background9bw.jpg, Background16.jpg)
- `agents/` — 9 green-teal-blue gradients
- `chladni-closeup/` — 15 landscape/nature photos for blur treatment
- `general/` — 7 blue-cyan gradients
- `creative/` — 1 coral sunset

### Rules (V2)
1. Title card, origin story, outro: FULL gradient, NO split with cover image
2. PillarDetail scenes: white left / gradient right with 2x2 frosted BrandedCard grid
3. PillarDetail description: fixed `height: 120px` for consistent centering across variants
4. "Your Focus" badge: absolutely positioned at bottom-left, NOT in flex flow
5. All v2 components live in `src/components/v2/`
