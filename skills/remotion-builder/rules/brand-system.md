# ElevenLabs Brand System for Remotion

## Color Palette (Monochrome Mode)

All ElevenLabs learning content uses monochrome mode. No color. Ever.

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

## Card Styling (Content Mode)

Content mode cards follow this pattern:

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

## BrandedCard Variant System (Hero Mode)

BrandedCard supports a `variant` prop that controls its visual treatment. Import: `import { BrandedCard, getCardColors } from "./BrandedCard"; import type { CardVariant } from "./BrandedCard";`

| Variant | Background | Blur | Text | Default? |
|---------|-----------|------|------|----------|
| `darkflat` | `rgba(0,0,0,0.3)` | NONE | White | **YES — this is the default** |
| `darkglass` | `rgba(0,0,0,0.3)` | `blur(20px)` | White | No — use for isolated cards only |
| `glass` | `rgba(255,255,255,0.08)` | `blur(40px)` | White | No |
| `baseline` | `rgba(245,243,241,0.88)` | `blur(24px)` | Dark | No — old v2 cream look |
| `outline` | transparent | NONE | White | No |
| `pill` | `rgba(255,255,255,0.12)` | `blur(30px)` | White | No |
| `acrylic` | `rgba(245,243,241,0.45)` | `blur(12px)` | Dark | No |
| `gradientborder` | `rgba(0,0,0,0.15)` | `blur(20px)` | White | No |

### When to use each variant
- **`darkflat`** — Default for all cards. Use everywhere unless you have a specific reason not to. Safe in dense grids (no blur artifacts).
- **`darkglass`** — Use for isolated, prominent cards (1-2 on screen). Do NOT use when 4+ cards are close together — causes rendering artifacts.
- **`glass`** — Subtle glass effect for understated cards.
- **`baseline`** — The old v2 frosted cream look. Use only when matching legacy compositions.
- **`outline`** — Transparent with border only. Good for secondary/supporting cards.
- **`pill`** — Compact pill-shaped elements, tags, badges.
- **`acrylic`** — Semi-translucent cream. Good for overlapping content.
- **`gradientborder`** — Cards with gradient-animated borders for emphasis.

### Card text colors via `getCardColors(variant)`

Never hardcode card text colors. Use the helper function:

```tsx
const colors = getCardColors("darkflat");
// Returns: { title, description, label, divider, decorativeNumber }
// darkflat/darkglass/glass/outline/pill/gradientborder → light (white) text colors
// baseline/acrylic → dark text colors
```

### Card layout rules
- **Cards should NOT stretch** — remove `flex: 1` from card styles. Cards should hug their content.
- **Parent container** should use `alignItems: "center"` to center cards vertically.
- **No backdrop-filter on dense grids** — when 4+ cards are close together, use `darkflat` (no blur) to avoid rendering artifacts.

### Icon rules on Hero scenes
- **All icons on Hero scenes must be white** — use `brand-assets/icons/white/` variants.
- For product icons that only come in black, apply `filter: "brightness(0) invert(1)"` to make them white.
- Inline SVG icons in card content should use `stroke="#FFFFFF"` not `stroke="#1E1916"` on Hero mode scenes.

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

### BrandedCard (Variant System)
Card component for Hero scenes with multiple visual variants. Component at `src/components/v2/BrandedCard.tsx`.
```tsx
import { BrandedCard, getCardColors } from "./BrandedCard";
import type { CardVariant } from "./BrandedCard";

// Default (darkflat) — no variant prop needed
<BrandedCard style={{ padding: "48px 40px" }}>{children}</BrandedCard>

// Explicit variant
<BrandedCard variant="darkglass" style={{ padding: "48px 40px" }}>{children}</BrandedCard>

// Get text colors for the variant
const colors = getCardColors("darkflat");
<div style={{ color: colors.title }}>Card Title</div>
<div style={{ color: colors.description }}>Card description text</div>
```
Default variant is `darkflat`: `rgba(0,0,0,0.3)` bg, no blur, white text. See the full variant table in the Card Styling section above.

### Hero Mode Colors
| Element | Value |
|---------|-------|
| Headlines | WHITE (`#FFFFFF`) |
| Labels | `rgba(255,255,255,0.6)` |
| Dividers | `rgba(255,255,255,0.3)` |
| Accent lines | `rgba(255,255,255,0.6)` |
| Card text | Use `getCardColors(variant)` — returns `{ title, description, label, divider, decorativeNumber }`. Do NOT hardcode `#151515` or `#666666`. |
| Bottom text / captions | 38px, weight 400, `rgba(255,255,255,0.75)`, `marginTop: 40` — NOT small footnote text |
| Decorative numbers | `rgba(30,25,22,0.06)` |
| Icons | Always white — use `brand-assets/icons/white/` or apply `filter: "brightness(0) invert(1)"` to product icons |
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
