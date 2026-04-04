# Brand Assets for Remotion

Centralized brand assets for ElevenLabs Academy Remotion video compositions. Sourced from the official ElevenLabs Figma brand kit, PPTX pitch decks, and local brand kit files.

**Visual catalogs:**
- `index.html` — Master asset catalog (backgrounds, icons, orbs, diagrams)
- `scene-catalog.html` — Interactive V1 vs V2 scene catalog (44 scenes, filterable by mode)

## Directory Structure

```
brand-assets/
├── CLAUDE.md               # This file
├── index.html              # Master catalog index - links to all sub-catalogs
├── scene-catalog.html      # Interactive V1 vs V2 scene catalog (44 scenes)
├── BRAND_GUIDELINES.md     # Official ElevenLabs Brand Guidelines 2025
├── color-tokens.json       # Color token definitions (brand, pitch deck, Echo design system)
├── icons/
│   ├── catalog.html        # Interactive icon catalog (search, filter, size toggle)
│   ├── cream-manifest.json # Full metadata for cream icons
│   ├── black-manifest.json # Full metadata for black icons
│   ├── white-manifest.json # Full metadata for white icons
│   ├── cream/              # 130 icons on #F5F3F1 cream background (PNG)
│   ├── black/              # 130 icons in black on transparent (PNG)
│   ├── white/              # 130 icons in white on transparent (PNG)
│   └── product/            # 10 product-specific icons (convai, dubbing, tts, etc.)
├── backgrounds/
│   ├── catalog.html        # Visual catalog of all backgrounds
│   ├── noise-texture.jpg   # Shared noise overlay (mix-blend-mode: overlay, 80% opacity)
│   ├── seamless-texture.jpg # Seamless texture (mix-blend-mode: soft-light)
│   ├── general/            # 4 gradient JPGs (2048x1152) + 3 Chladni patterns (800x800)
│   ├── agents/             # 9 files - green/teal/blue palette variants (Figma + PPTX)
│   ├── chladni-closeup/    # 15 JPGs - Chladni Close-Up base layers (landscape/nature photography)
│   │   └── catalog.html    # Visual catalog of Chladni closeup backgrounds
│   ├── gradient/           # 16 JPGs - Atmospheric gradient backgrounds (Background0-15.jpg)
│   └── creative/           # 1 PNG (1598x899) - coral-sunset warm palette
├── voice-orbs/
│   ├── catalog.html        # Visual catalog of all orbs
│   └── 8 JPGs              # All 1174x1038, on black backgrounds
├── diagrams/
│   └── agents-platform-architecture.jpg  # Concentric rings diagram (2048x1152)
└── ui-highlights/
    └── phone-agent-live-activity.jpg     # Phone agent live activity UI (2048x1150)
```

## Asset Sources

| Folder | Source | Notes |
|--------|--------|-------|
| `icons/cream/`, `icons/black/`, `icons/white/` | Google Drive brand kit (`CoWork/09 - Branding/01_Style_Elements/5_Icons/`) | 130 icons per variant, PNG |
| `icons/product/` | Figma brand kit | 10 product-specific icons |
| `backgrounds/general/` | PPTX pitch deck exports + brand kit Chladni patterns | 4 gradient JPGs + 3 Chladni JPGs |
| `backgrounds/agents/` | Figma brand kit + PPTX pitch deck | 9 agents-branded gradient/nature variants |
| `backgrounds/chladni-closeup/` | Figma Brand Design System (XMegCFgEe0wVDrj5iecjwy) | 15 Chladni Close-Up base layers (landscape/nature photography) |
| `backgrounds/noise-texture.jpg` | Figma brand kit | Shared noise overlay texture |
| `backgrounds/seamless-texture.jpg` | Figma brand kit | Shared seamless texture |
| `backgrounds/gradient/` | Brand kit atmospheric gradients | 16 high-res gradient backgrounds (Background0-15.jpg) |
| `backgrounds/creative/` | PPTX pitch deck export | Creative-branded coral-sunset gradient |
| `voice-orbs/` | Figma brand kit export | 8 color variants, all on black backgrounds |
| `diagrams/` | PPTX pitch deck export | Platform architecture concentric rings |
| `ui-highlights/` | PPTX pitch deck export | Phone agent live activity screenshot |
| `color-tokens.json` | Echo Design System + brand kit + pitch deck CSS analysis | Token definitions and Figma source refs |

## Figma Source References

All Figma assets come from the ElevenLabs brand kit files. Key section IDs for future re-pulls at higher quality:

### Icons
- `section id="2001:2392" name="Icons_Cream"` - Cream background icons (130)
- `section id="2031:1283" name="Icons_Black_Transparent_BG"` - Black icons (130)
- `section id="3:6827" name="Icons_White_Transparent_BG"` - White icons (130)
- `section id="3:6208" name="Icons_Color"` - Color icons
- `section id="4:717" name="ElevenLabs+ElevenReader_Icons"` - Product icons

### Backgrounds
- `section id="2036:1223" name="Backgrounds to Export"` - General gradients
- `section id="2402:1871" name="Backgrounds_Agents"` - Agents branded
- `section id="2402:1701" name="Backgrounds_Creative"` - Creative branded
- **Brand Design System file** `XMegCFgEe0wVDrj5iecjwy` - Chladni Close-Up base layers

### Voice Orbs
- `section id="3:6207" name="Voice_Orbs"` - Standard orbs (black background)
- `section id="2014:1493" name="Voice_Orbs"` - Additional variants
- `section id="2014:1580" name="Voice_Orbs"` - More variants
- `section id="2020:5337" name="Voice_Orbs_transparent_bg"` - Transparent background set

### Figma File Metadata
- **Elements Library file** timestamp: `1775138761551`
- **Elements file** timestamp: `1775226662225`
- **Echo Design System library key:** `lk-6312ad2f8c882e5dd25ff27d662ac247369e4b2be81dc4ebf8384213428e0c99404c13b2b23b557ddf09021f9b5ddf2f3c6396a069dcf9c29728a25f44d268ab`

## Exporting Assets from Figma

When you need new assets from the Figma Brand Design System:

1. **Preview first:** Use `get_screenshot` (Figma MCP) to confirm you have the right node
2. **For composited frames** (backgrounds with blur+noise baked in): Export from Figma desktop — the MCP return channel truncates files >20KB. Select the frame → Export → JPG at 1x → save to `public/brand-assets/`
3. **For raw source images** (icons, logos, diagrams): Use `get_design_context` (Figma MCP) which returns download URLs → `curl` them to save locally
4. **For small assets** (<20KB): Use `use_figma` with `exportAsync` → return base64 → decode and save

**Key Figma files:**
- Brand Design System (External): `0AzA12gk3NEAqRLwRKnAPc`
- Background frames are in the `↪ Elements Library` page

## Using Assets in Remotion

All assets are in the `public/` directory and accessed via `staticFile()`:

```tsx
import { Img, staticFile } from 'remotion';

// Backgrounds
<Img src={staticFile('brand-assets/backgrounds/agents/agents-bg-01-green-teal.png')} />
<Img src={staticFile('brand-assets/backgrounds/agents/agents-bg-00-green-white.jpg')} />
<Img src={staticFile('brand-assets/backgrounds/general/general-bg-01-blue-cyan-chladni.jpg')} />
<Img src={staticFile('brand-assets/backgrounds/creative/creative-bg-01-coral-sunset.png')} />
<Img src={staticFile('brand-assets/backgrounds/chladni-closeup/closeup-01-red-rock-blue-sky.jpg')} />

// Gradient backgrounds
<Img src={staticFile('brand-assets/backgrounds/gradient/Background0.jpg')} />

// Shared textures (overlay on top of backgrounds)
<Img src={staticFile('brand-assets/backgrounds/noise-texture.jpg')}
     style={{ mixBlendMode: 'overlay', opacity: 0.8 }} />
<Img src={staticFile('brand-assets/backgrounds/seamless-texture.jpg')}
     style={{ mixBlendMode: 'soft-light' }} />

// Icons
<Img src={staticFile('brand-assets/icons/cream/015-zap-lightning-flash-thunder.png')} />
<Img src={staticFile('brand-assets/icons/black/015-zap-lightning-flash-thunder.png')} />
<Img src={staticFile('brand-assets/icons/white/015-zap-lightning-flash-thunder.png')} />
<Img src={staticFile('brand-assets/icons/product/convai.png')} />

// Voice Orbs
<Img src={staticFile('brand-assets/voice-orbs/orb-teal-on-black.jpg')} />

// Diagrams & UI Highlights
<Img src={staticFile('brand-assets/diagrams/agents-platform-architecture.jpg')} />
<Img src={staticFile('brand-assets/ui-highlights/phone-agent-live-activity.jpg')} />
```

## Two Scene Modes

Remotion compositions use two visual modes depending on the scene's purpose:

### Hero Mode
Full-bleed gradient background image with white text and frosted cream cards.

```tsx
// Background: gradient image covering the full frame
<AbsoluteFill>
  <Img src={staticFile('brand-assets/backgrounds/agents/agents-bg-01-green-teal.png')}
       style={{ width: '100%', height: '100%', objectFit: 'cover' }} />
</AbsoluteFill>

// Text: WHITE on gradient
<div style={{ color: '#FFFFFF', fontWeight: 700 }}>Hero Headline</div>

// Cards: frosted cream with backdrop blur
<div style={{
  backgroundColor: 'rgba(245, 243, 241, 0.92)',  // CREAM at 92% opacity
  backdropFilter: 'blur(20px)',
  borderRadius: 16,
  padding: 32,
}}>
  <div style={{ color: '#1E1916' }}>Card content in GRAPHITE</div>
</div>
```

**When to use:** Opening/closing scenes, module intros, dramatic emphasis, visual breaks between content sections.

### Content Mode
Clean light background with dark text and solid cream cards.

```tsx
// Background: OFF_WHITE
<AbsoluteFill style={{ backgroundColor: '#FAFAFA' }} />

// Text: GRAPHITE on light
<div style={{ color: '#1E1916', fontWeight: 700 }}>Content Headline</div>

// Cards: solid cream
<div style={{
  backgroundColor: '#F5F3F1',  // CREAM solid
  borderRadius: 16,
  padding: 32,
}}>
  <div style={{ color: '#1E1916' }}>Card content in GRAPHITE</div>
</div>
```

**When to use:** Instructional content, feature breakdowns, step-by-step walkthroughs, most of the lesson runtime.

## Brand Colors

| Name | Hex | Use |
|------|-----|-----|
| GRAPHITE | `#1E1916` | Primary text, headings |
| OFF_WHITE | `#FAFAFA` | Scene backgrounds (Content mode) |
| CREAM | `#F5F3F1` | Card backgrounds, accent areas, icon backgrounds |
| LIGHT_GRAY | `#E0DEDC` | Borders, dividers |
| MID_GRAY | `#999999` | Secondary text, labels |
| NEAR_BLACK | `#151515` | Icon fill on cream cards, dark mode text |
| AMBER | `#F59E0B` | Annotations, callout markers, highlights |

**Design rules:**
- Content mode: OFF_WHITE backgrounds, GRAPHITE text, CREAM cards
- Hero mode: gradient bg image, WHITE text, frosted CREAM cards (rgba 0.92 + backdrop-blur)
- GRAPHITE is for text only, never as a background fill
- See `color-tokens.json` for the full token set including Echo Design System tokens and pitch deck colors

## Voice Orbs

8 color variants, all 1174x1038px JPGs on black backgrounds:
- `orb-teal-on-black.jpg`
- `orb-purple-blue-on-black.jpg`
- `orb-pink-on-black.jpg`
- `orb-coral-on-black.jpg`
- `orb-green-on-black.jpg`
- `orb-gold-on-black.jpg`
- `orb-hotpink-on-black.jpg`
- `orb-lavender-on-black.jpg`

These have a metallic radial sheen - textured spheres that are the signature ElevenLabs visual element. Use as animated accent elements in compositions (scale, rotate, pulse, parallax float). The black background makes them easy to composite with `mix-blend-mode: screen` or `lighten` to knock out the black.

## Background Files

### General (7 files)
| File | Dimensions | Type |
|------|-----------|------|
| `general-bg-01-blue-cyan-chladni.jpg` | 2048x1152 | Gradient with Chladni texture |
| `general-bg-02-dark-blue-teal.jpg` | 2048x1152 | Dark blue-teal gradient |
| `general-bg-03-blue-cyan-clean.jpg` | 2048x1152 | Clean blue-cyan gradient |
| `general-bg-04-teal-gradient.jpg` | 2048x1150 | Teal gradient |
| `image38-chladni-pattern.jpg` | 800x800 | Standalone Chladni pattern |
| `image42-chladni-pattern.jpg` | 800x800 | Standalone Chladni pattern |
| `image50-chladni-pattern.jpg` | 800x800 | Standalone Chladni pattern |

### Agents (9 files)
| File | Dimensions | Type |
|------|-----------|------|
| `agents-bg-00-green-white.jpg` | JPG | Green-white gradient (Figma, base layer) |
| `agents-bg-01-blue-green.jpg` | JPG | Blue-green gradient (Figma) |
| `agents-bg-01-green-teal.png` | 1920x1080 | Green-teal-blue gradient (PPTX, original) |
| `agents-bg-02-blue-warm.jpg` | JPG | Blue-warm gradient (Figma) |
| `agents-bg-03-dark-green.jpg` | JPG | Dark green gradient (Figma) |
| `agents-bg-04-teal-blue.jpg` | JPG | Teal-blue gradient (Figma) |
| `agents-bg-05-grass-warm.jpg` | JPG | Grass-warm gradient (Figma) |
| `agents-bg-06-flowers-dark.jpg` | JPG | Flowers-dark (Figma) |
| `agents-bg-06-layer2.jpg` | JPG | Layer 2 variant (Figma) |

### Chladni Close-Up (15 files)

These are base image layers from the Brand Design System Figma file (XMegCFgEe0wVDrj5iecjwy). They are landscape/nature photography that gets blur + noise + Chladni curve overlay treatment in the final composite. **In Remotion, apply blur (backdrop-filter: blur(80px)) and noise overlay programmatically.**

| File | Description |
|------|-------------|
| `closeup-01-red-rock-blue-sky.jpg` | Red rock with blue sky |
| `closeup-02-blue-coral-splash.jpg` | Blue coral splash |
| `closeup-03-blue-mountain-orange.jpg` | Blue mountain with orange |
| `closeup-04-blue-peach-dunes.jpg` | Blue-peach dunes |
| `closeup-05-green-leaves.jpg` | Green leaves |
| `closeup-06-warm-abstract.jpg` | Warm abstract |
| `closeup-07-pink-teal-blend.jpg` | Pink-teal blend |
| `closeup-08-dreamy-clouds.jpg` | Dreamy clouds |
| `closeup-09-water-splash-blue.jpg` | Water splash blue |
| `closeup-10-mountains-motion.jpg` | Mountains in motion |
| `closeup-11-flowers-blur-warm.jpg` | Flowers blur warm |
| `closeup-12-flowers-purple-pink.jpg` | Flowers purple-pink |
| `closeup-13-blue-purple-portrait.jpg` | Blue-purple portrait |
| `closeup-14-blue-red-portrait.jpg` | Blue-red portrait |
| `closeup-15-landscape-green.jpg` | Landscape green |

### Shared Textures (2 files)

These are overlay textures that apply on top of any background using CSS blend modes.

| File | Usage |
|------|-------|
| `noise-texture.jpg` | Noise overlay - apply with `mix-blend-mode: overlay` at 80% opacity |
| `seamless-texture.jpg` | Seamless texture - apply with `mix-blend-mode: soft-light` |

### Gradient (16 files)

Atmospheric gradient backgrounds from the brand kit. High-res JPGs suitable for full-bleed Hero mode scenes. These are clean gradients without embedded screenshots or UI elements.

| File | Description |
|------|-------------|
| `Background0.jpg` | Gradient 0 |
| `Background1.jpg` | Gradient 1 |
| `Background2.jpg` | Gradient 2 |
| `Background3.jpg` | Gradient 3 |
| `Background4.jpg` | Gradient 4 |
| `Background5.jpg` | Gradient 5 |
| `Background6.jpg` | Gradient 6 |
| `Background7.jpg` | Gradient 7 |
| `Background8.jpg` | Gradient 8 |
| `Background9.jpg` | Gradient 9 |
| `Background10.jpg` | Gradient 10 |
| `Background11.jpg` | Gradient 11 |
| `Background12.jpg` | Gradient 12 |
| `Background13.jpg` | Gradient 13 |
| `Background14.jpg` | Gradient 14 |
| `Background15.jpg` | Gradient 15 |
| `Background16.jpg` | Gray composited gradient (from Figma Brand Design System) |
| `Background9bw.jpg` | Gray composited gradient (from Figma Brand Design System) |

### Creative (1 file)
| File | Dimensions | Type |
|------|-----------|------|
| `creative-bg-01-coral-sunset.png` | 1598x899 | Coral-sunset warm gradient, creative-branded |

## Icons

### What's Here
- **130 brand icons** in 3 color variants (cream, black, white) = 390 total icon files
- **10 product icons** (convai, dubbing, home, sfx, stt, studio, tts, voice changer, voice isolator, voices)
- All icons are PNG format

### Color Variants
| Variant | Background | Icon Color | Best For |
|---------|-----------|------------|----------|
| `cream/` | #F5F3F1 cream | #151515 dark | Dark composition backgrounds, feature cards on dark |
| `black/` | Transparent | #1E1916 black | Light composition backgrounds, OFF_WHITE scenes |
| `white/` | Transparent | #FFFFFF white | Dark composition backgrounds, overlays |
| `product/` | Transparent | Black | Product-specific callouts |

### Naming Convention
Icons use the format: `{idx:03d}-{clean-name}.png`
- `idx` is a zero-padded 3-digit sequential index (000-129)
- `clean-name` is derived from the Figma component name
- Examples: `000-calendar-clock-4.png`, `015-zap-lightning-flash-thunder.png`

### Finding the Right Icon
1. Open `icons/catalog.html` in a browser
2. Use the search bar to filter by name/keywords
3. Switch between Cream/Black/White tabs to see variants
4. Click any icon to copy its file path
