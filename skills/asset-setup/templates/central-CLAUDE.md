# ElevenLabs Brand Kit — Central Asset Store

This directory is the central, shared store for ElevenLabs brand assets. It is managed by the [elevenlabs-brand-kit](https://github.com/jakeat11labs/elevenlabs-brand-kit) Claude Code plugin.

## What's Here

- `brand-assets/` — backgrounds, icons, voice orbs, logos, diagrams, screenshots, color tokens, brand guidelines, and visual catalogs
- `brand-assets/fonts/` — KMR Waldenburg typeface (Buch 300, Normal 400, Halbfett 600, Fett 700)
- `config.json` — install metadata (version, storage mode, last updated)
- `AGENTS.md` — universal agent guide for brand compliance (framework-agnostic)

Projects using central storage have `public/brand-assets` and `public/fonts` symlinked here.

## Brand Identity

ElevenLabs is a voice AI company. The visual identity is **minimal, monochrome, and warm** — color comes from imagery, not UI palettes. Two visual modes:

- **Core Brand** — gradient imagery backgrounds, white text, darkflat cards. For marketing, educational, product, training, and onboarding content.
- **Monochrome** — black/white/gray only. Reserved strictly for developer API documentation and technical reference.

## Brand Rules (Non-Negotiable)

1. **Name:** Always "ElevenLabs" — one word, capital E, capital L. Never "Eleven Labs", "elevenlabs", "ELEVENLABS", or with "II" prefix in text.
2. **Font:** KMR Waldenburg exclusively. Two weights: Book (light/refined) and Normal (bold/confident). Fallback: Inter or Helvetica Neue.
3. **Colors:** Graphite (#1E1916) for text. Off-White (#FAFAFA) for backgrounds. Cream (#F5F3F1) for cards. No gray with color accents.
4. **Logo:** Black or white only. Never colored, rotated, shadowed, outlined, or distorted. Safespace = logo height on all sides.
5. **Imagery:** Saturated, dreamlike, atmospheric. Always treated with noise (70% opacity overlay) and blur.
6. **Chladni patterns:** Core visual element from sound visualization. Used as overlays, masks, and standalone graphics.

## Color Tokens

| Name | Hex | Use |
|------|-----|-----|
| Graphite | #1E1916 | Primary text, headings (text only, never backgrounds) |
| Off-White | #FAFAFA | Scene/page backgrounds |
| Cream | #F5F3F1 | Card backgrounds, accent areas |
| Light-Gray | #E0DEDC | Borders, dividers |
| Mid-Gray | #999999 | Secondary text, labels |
| Near-Black | #151515 | Icon fill on cream cards |
| Amber | #F59E0B | Annotations, callout markers |

## Typography Scale

| Use | Weight | Size | Line-Height | Tracking |
|-----|--------|------|-------------|----------|
| Hero headline | Book | 80px | 100% | 0% |
| Large headline | Normal | 58px | 100% | -2% |
| Body / description | Book | 27px | 150% | 0% |
| Body small | Book | 18px | 140% | 0% |

## Asset Paths

When central storage is active, resolve assets from this directory:

```
~/.elevenlabs-kit/brand-assets/backgrounds/   # 52+ background images
~/.elevenlabs-kit/brand-assets/icons/          # 400+ icons (cream/black/white + product)
~/.elevenlabs-kit/brand-assets/voice-orbs/     # 8 metallic sphere variants
~/.elevenlabs-kit/brand-assets/logos/           # Black + white, PNG + SVG
~/.elevenlabs-kit/brand-assets/fonts/          # KMR Waldenburg OTFs
~/.elevenlabs-kit/brand-assets/color-tokens.json
~/.elevenlabs-kit/brand-assets/BRAND_GUIDELINES.md
~/.elevenlabs-kit/brand-assets/index.html      # Visual asset catalog — open in browser
```

## Icon Selection

| Context | Variant |
|---------|---------|
| Dark/gradient background (Hero mode) | `icons/white/` — always |
| Light background (Content mode) | `icons/black/` |
| Dark compositions, feature cards on dark | `icons/cream/` |
| Product callouts on dark backgrounds | `icons/product/` + `filter: brightness(0) invert(1)` |

## Voice Orbs

8 metallic spheres (teal, purple, pink, coral, green, gold, hotpink, lavender) on black backgrounds. Composite with `mix-blend-mode: screen` to knock out the black. Use as animated accent elements.

## Two Visual Modes in Detail

### Hero Mode (dark, dramatic)
- Full-bleed gradient background image
- WHITE text on gradient
- Cards use `darkflat` variant (default) — use `darkflat` for dense grids (4+ cards)
- Icons: always `icons/white/`
- Card variants available: `darkflat`, `darkglass`, `glass`, `baseline`, `outline`, `pill`, `acrylic`, `gradientborder`

### Content Mode (light, clean)
- Off-White (#FAFAFA) background
- Graphite (#1E1916) text
- Cream (#F5F3F1) solid cards
- Icons: `icons/black/`

## Brand Compliance

Run `/elevenlabs-brand-kit:brand` to check any content against these guidelines.
Run `/elevenlabs-brand-kit:asset-setup` to download or update assets.

## Full Guidelines

See `brand-assets/BRAND_GUIDELINES.md` for the complete 2025 Brand Design System reference (extracted from the official 59-slide Figma file).
