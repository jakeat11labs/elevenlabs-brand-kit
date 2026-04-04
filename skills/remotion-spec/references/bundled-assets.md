# Brand Assets — Full Inventory

This document covers all ElevenLabs brand assets available for Remotion video production: both the assets bundled with this skill and the expanded library in the Remotion project.

## Asset Locations

| Location | Contents |
|----------|----------|
| **Skill assets** (`assets/`) | Logos, II icons, fonts, Chladni covers, gradient backgrounds |
| **Remotion project** (`public/brand-assets/`) | 34 backgrounds, 8 voice orbs, 400+ icons, diagrams, color tokens, textures |

The Remotion project path is `/Users/jakerains/Projects/remotion/`. Assets in `public/brand-assets/` are referenced with `staticFile("brand-assets/...")` in code.

---

## Logos (`assets/logos/`)

| File | Format | Use |
|------|--------|-----|
| `Logo-black.png` | PNG (1988x257) | Full wordmark on light backgrounds |
| `Logo-white.png` | PNG | Full wordmark on dark/Hero backgrounds |
| `ElevenLabs Logo_black.svg` | SVG | Full wordmark on light (scalable) |
| `ElevenLabs Logo_white.svg` | SVG | Full wordmark on dark/Hero (scalable) |

## II Icon (`assets/ii-icon/`)

| File | Format | Use |
|------|--------|-----|
| `ElevenLabs_Icon_Black.png` | PNG (135x216) | II symbol on Content scenes |
| `ElevenLabs_Icon_White.png` | PNG | II symbol on Hero scenes |
| `ElevenLabs_Icon_Black.svg` | SVG | II symbol on Content (scalable) |
| `ElevenLabs_Icon_White.svg` | SVG | II symbol on Hero (scalable) |

## Fonts (`assets/fonts/`)

| File | Weight | Use |
|------|--------|-----|
| `KMR-Waldenburg-Buch.otf` | Book (300) | Large headlines, body text, subheads |
| `KMR-Waldenburg-Normal.otf` | Normal (400) | Medium headlines, emphasis |
| `KMR-Waldenburg-Halbfett.otf` | Semibold (600) | Strong emphasis, labels |
| `KMR-Waldenburg-Fett.otf` | Bold (700) | Maximum emphasis |

## Chladni Covers (`assets/covers/`)

| File | Dimensions | Description |
|------|-----------|-------------|
| `Cover0.jpg` - `Cover3.jpg` | 1500x500 or 1584x396 | Chladni pattern banner images |

Use as background textures on title/outro scenes, Chladni overlays, or as masks.

## Gradient Backgrounds (`assets/gradients/`)

| File | Use |
|------|-----|
| `Background0.jpg` - `Background2.jpg` | Atmospheric gradient backgrounds for Hero scenes |

---

## Expanded Asset Library (`public/brand-assets/`)

All paths below are relative to the Remotion project's `public/brand-assets/` directory. Reference with `staticFile("brand-assets/...")` in Remotion code.

### Backgrounds (34 images + 2 textures)

#### `backgrounds/agents/` (9 images)
Agent-themed backgrounds. Green, teal, blue tones. Best for agent-related content, platform scenes.

| File | Description |
|------|-------------|
| `agents-bg-00-green-white.jpg` | Green gradient on white |
| `agents-bg-01-blue-green.jpg` | Blue-green blend |
| `agents-bg-01-green-teal.png` | Green-teal (PNG) |
| `agents-bg-02-blue-warm.jpg` | Blue with warm tones |
| `agents-bg-03-dark-green.jpg` | Dark green atmospheric |
| `agents-bg-04-teal-blue.jpg` | Teal-blue gradient |
| `agents-bg-05-grass-warm.jpg` | Warm grass tones |
| `agents-bg-06-flowers-dark.jpg` | Dark floral |
| `agents-bg-06-layer2.jpg` | Layer 2 variant for complex blur |

#### `backgrounds/chladni-closeup/` (15 images)
Diverse saturated photography with natural blur. Best for concept reveals, emotional moments, methodology scenes.

| File | Description |
|------|-------------|
| `closeup-01-red-rock-blue-sky.jpg` | Red rock, blue sky |
| `closeup-02-blue-coral-splash.jpg` | Blue coral splash |
| `closeup-03-blue-mountain-orange.jpg` | Blue mountain, orange |
| `closeup-04-blue-peach-dunes.jpg` | Blue peach dunes |
| `closeup-05-green-leaves.jpg` | Green leaves |
| `closeup-06-warm-abstract.jpg` | Warm abstract |
| `closeup-07-pink-teal-blend.jpg` | Pink-teal blend |
| `closeup-08-dreamy-clouds.jpg` | Dreamy clouds |
| `closeup-09-water-splash-blue.jpg` | Water splash blue |
| `closeup-10-mountains-motion.jpg` | Mountains in motion |
| `closeup-11-flowers-blur-warm.jpg` | Warm blurred flowers |
| `closeup-12-flowers-purple-pink.jpg` | Purple-pink flowers |
| `closeup-13-blue-purple-portrait.jpg` | Blue-purple portrait |
| `closeup-14-blue-red-portrait.jpg` | Blue-red portrait |
| `closeup-15-landscape-green.jpg` | Green landscape |

#### `backgrounds/general/` (7 images)
Blue/cyan/teal gradients, some with Chladni pattern overlays. Versatile for title cards, generic hero moments.

| File | Description |
|------|-------------|
| `general-bg-01-blue-cyan-chladni.jpg` | Blue-cyan with Chladni |
| `general-bg-02-dark-blue-teal.jpg` | Dark blue-teal |
| `general-bg-03-blue-cyan-clean.jpg` | Clean blue-cyan |
| `general-bg-04-teal-gradient.jpg` | Teal gradient |
| `image38-chladni-pattern.jpg` | Chladni pattern tile |
| `image42-chladni-pattern.jpg` | Chladni pattern tile |
| `image50-chladni-pattern.jpg` | Chladni pattern tile |

#### `backgrounds/creative/` (1 image)
| File | Description |
|------|-------------|
| `creative-bg-01-coral-sunset.png` | Coral sunset (PNG) |

#### Shared Textures
| File | Use |
|------|-----|
| `backgrounds/noise-texture.jpg` | Noise overlay for all scenes (70% opacity, overlay blend) |
| `backgrounds/seamless-texture.jpg` | Seamless tiling texture |

### Voice Orbs (`voice-orbs/`, 8 images)

Metallic orb images on black backgrounds. Use as atmospheric accents on Hero scenes.

| File | Color |
|------|-------|
| `orb-teal-on-black.jpg` | Teal |
| `orb-purple-blue-on-black.jpg` | Purple-blue |
| `orb-pink-on-black.jpg` | Pink |
| `orb-coral-on-black.jpg` | Coral |
| `orb-green-on-black.jpg` | Green |
| `orb-gold-on-black.jpg` | Gold |
| `orb-hotpink-on-black.jpg` | Hot pink |
| `orb-lavender-on-black.jpg` | Lavender |

**Compositing tip:** Voice orbs are on black backgrounds. Use `mix-blend-mode: screen` to composite them over other content — the black disappears and only the orb remains.

```tsx
<Img
  src={staticFile("brand-assets/voice-orbs/orb-teal-on-black.jpg")}
  style={{
    position: "absolute",
    width: 400,
    height: 400,
    mixBlendMode: "screen",
    opacity: 0.7,
  }}
/>
```

### Icons (`icons/`, 400+ icons)

Four variant directories, each containing ~130 icons:

| Directory | Count | Use |
|-----------|-------|-----|
| `icons/black/` | 130 | Icons on Content scenes (light backgrounds) |
| `icons/white/` | 130 | Icons on Hero scenes (gradient backgrounds) |
| `icons/cream/` | 130 | Icons on dark surfaces or as accents |
| `icons/product/` | 10 | Product-specific icons (convai, dubbing, home, sfx, stt, etc.) |

Icon files are named numerically: `000-camera-icon.png`, `001-checkmark-icon.png`, etc. Check manifests (`black-manifest.json`, `cream-manifest.json`, `white-manifest.json`) for the full index.

```tsx
// Black icon on Content scene
<Img src={staticFile("brand-assets/icons/black/001-checkmark-icon.png")} style={{ width: 48, height: 48 }} />

// White icon on Hero scene
<Img src={staticFile("brand-assets/icons/white/001-checkmark-icon.png")} style={{ width: 48, height: 48 }} />

// Product icon
<Img src={staticFile("brand-assets/icons/product/convai.png")} style={{ width: 64, height: 64 }} />
```

### Diagrams (`diagrams/`, 1 image)

| File | Description |
|------|-------------|
| `agents-platform-architecture.jpg` | ElevenAgents platform architecture diagram |

### Color Tokens (`color-tokens.json`)

Machine-readable color definitions sourced from the Echo Design System, Brand Kit, and Agents Master Pitch deck. Includes brand tokens (GRAPHITE, OFF_WHITE, CREAM, etc.), pitch deck colors, and Echo Design System static/semantic tokens.

---

## Background Compositing Pipeline (Hero Mode)

When a Hero scene uses a background image, the compositing stack from back to front is:

1. **Background image** — Full-bleed, `object-fit: cover`, fills the 1920x1080 frame
2. **Blur layer** — Simple blur (25) or Complex blur (180 + 0.1 reveal)
3. **Noise overlay** — `noise-texture.jpg` at 70% opacity, overlay blend mode
4. **Optional Chladni overlay** — Cover image or SVG pattern at low opacity
5. **Content layer** — Text, cards, icons, watermark

```tsx
// Example Hero scene background stack
<AbsoluteFill>
  {/* 1. Background image */}
  <Img
    src={staticFile("brand-assets/backgrounds/agents/agents-bg-01-blue-green.jpg")}
    style={{
      width: "100%",
      height: "100%",
      objectFit: "cover",
      filter: "blur(25px)",
    }}
  />

  {/* 2. Noise overlay */}
  <Img
    src={staticFile("brand-assets/backgrounds/noise-texture.jpg")}
    style={{
      position: "absolute",
      width: "100%",
      height: "100%",
      objectFit: "cover",
      opacity: 0.7,
      mixBlendMode: "overlay",
    }}
  />

  {/* 3. Content on top */}
  <div style={{ position: "relative", zIndex: 1 }}>
    {/* Cards, text, watermark go here */}
  </div>
</AbsoluteFill>
```

---

## How to Use in a Remotion Project

### Step 1: Copy skill assets to project (if not already present)

```bash
# From the skill's assets directory, copy what you need to public/
cp assets/logos/Logo-black.png my-remotion-project/public/
cp assets/logos/Logo-white.png my-remotion-project/public/
cp assets/ii-icon/ElevenLabs_Icon_Black.svg my-remotion-project/public/
cp assets/ii-icon/ElevenLabs_Icon_White.svg my-remotion-project/public/
cp assets/fonts/*.otf my-remotion-project/public/fonts/
cp assets/covers/Cover0.jpg my-remotion-project/public/
```

### Step 2: Load fonts in Remotion

```tsx
import { loadFont } from "@remotion/fonts";
import { staticFile } from "remotion";

// Load KMR Waldenburg — the actual brand font
const fontLoaded = loadFont({
  family: "KMR Waldenburg",
  url: staticFile("fonts/KMR-Waldenburg-Buch.otf"),
  weight: "300",
});

const fontBold = loadFont({
  family: "KMR Waldenburg",
  url: staticFile("fonts/KMR-Waldenburg-Normal.otf"),
  weight: "400",
});

// Then use in components:
<div style={{ fontFamily: "'KMR Waldenburg', 'Inter', sans-serif" }}>
```

### Step 3: Use logos and icons

```tsx
import { Img, staticFile } from "remotion";

// Full wordmark — choose variant based on scene mode
<Img src={staticFile("Logo-black.png")} style={{ height: 32 }} />   // Content mode
<Img src={staticFile("Logo-white.png")} style={{ height: 32 }} />   // Hero mode

// II icon — choose variant based on scene mode
<Img src={staticFile("ElevenLabs_Icon_Black.svg")} style={{ height: 48 }} />  // Content mode
<Img src={staticFile("ElevenLabs_Icon_White.svg")} style={{ height: 48 }} />  // Hero mode
```

### Step 4: Use background images for Hero scenes

```tsx
import { Img, staticFile } from "remotion";

// Full Hero background with blur + noise
<AbsoluteFill>
  <Img
    src={staticFile("brand-assets/backgrounds/chladni-closeup/closeup-08-dreamy-clouds.jpg")}
    style={{
      width: "100%",
      height: "100%",
      objectFit: "cover",
      filter: "blur(25px)",
    }}
  />
  <Img
    src={staticFile("brand-assets/backgrounds/noise-texture.jpg")}
    style={{
      position: "absolute",
      width: "100%",
      height: "100%",
      objectFit: "cover",
      opacity: 0.7,
      mixBlendMode: "overlay",
    }}
  />
</AbsoluteFill>
```

### Step 5: Use Chladni covers as overlays

```tsx
import { Img, staticFile } from "remotion";

<Img
  src={staticFile("Cover0.jpg")}
  style={{
    position: "absolute",
    width: "100%",
    height: "100%",
    objectFit: "cover",
    opacity: 0.08, // Keep subtle
  }}
/>
```

---

## When to Use Real Assets vs. Code-Generated

| Element | Real Asset | Code-Generated |
|---------|-----------|----------------|
| Full wordmark (II + ElevenLabs) | **Always use the real logo files** | Never recreate |
| II icon standalone | Use SVG/PNG when available | OK to build as two `<div>` bars for simple cases |
| KMR Waldenburg font | **Use the OTF files** when possible | Fall back to Inter only if OTF can't be loaded |
| Chladni patterns | Use cover JPGs or closeup backgrounds | SVG component for animated/generative overlays |
| Gradient backgrounds | **Use brand-assets/backgrounds/** for Hero scenes | Not needed for Content mode |
| Voice orbs | **Use brand-assets/voice-orbs/** always | Never recreate — the metallic texture requires the real image |
| Icons | **Use brand-assets/icons/** always | Never recreate |
| Noise texture | **Use noise-texture.jpg** when available | CSS/canvas noise as fallback only |

## Brand Font Priority

1. **KMR Waldenburg** (bundled OTF) — always try this first
2. **Inter** — fallback when KMR can't be loaded
3. **Helvetica Neue** — emergency fallback
