# ElevenLabs Video Brand Checklist (v2)

Run through this before delivering any video. Every item must pass.

## Two-Mode Validation

### Hero Mode Scenes
- [ ] Background is a full-bleed image from `brand-assets/backgrounds/`
- [ ] Background image has blur applied (Simple: 25, or Complex: 180 + 0.1 reveal)
- [ ] Noise overlay present (70% opacity, overlay blend mode)
- [ ] All text is WHITE (#FFFFFF)
- [ ] Cards use frosted cream style: `rgba(245, 243, 241, 0.88)`, `backdrop-filter: blur(24px)`, border `1px solid rgba(255,255,255,0.12)`, radius 14px
- [ ] II watermark is WHITE at 25% opacity
- [ ] No GRAPHITE or DARK_GRAY text on Hero scenes
- [ ] No solid OFF_WHITE backgrounds on Hero scenes

### Content Mode Scenes
- [ ] Background is solid OFF_WHITE (#FAFAFA)
- [ ] Headlines use GRAPHITE (#1E1916)
- [ ] Body text uses DARK_GRAY (#555555)
- [ ] Cards use solid CREAM (#F5F3F1) with LIGHT_GRAY (#E0DEDC) borders
- [ ] II watermark is GRAPHITE at 35% opacity
- [ ] No background images on Content scenes
- [ ] No WHITE text on Content scenes

### Mode Alternation
- [ ] Title card (Scene 1) is Hero mode
- [ ] Outro (last scene) is Hero mode
- [ ] Hero and Content scenes alternate where natural for visual rhythm
- [ ] No more than 3 consecutive scenes in the same mode (unless content demands it)

## Naming
- [ ] "ElevenLabs" spelled correctly everywhere (one word, capital E and L)
- [ ] No "II" prefix in written text (II is only the visual logo/icon)
- [ ] Product names correct: ElevenCreative, ElevenAgents, ElevenAPI

## Colors
- [ ] Color comes from images (Hero mode), not arbitrary color palettes
- [ ] No gray shades mixed with color accents (per brand guidelines)
- [ ] GRAPHITE (#1E1916) used as warm dark, never pure black for text
- [ ] CREAM (#F5F3F1) used for card fills (frosted on Hero, solid on Content)
- [ ] No random colors — all from the defined palette

## Typography
- [ ] KMR Waldenburg as primary font (Inter as fallback only)
- [ ] Hero headlines: 80px, Book weight (300), tracking -3%
- [ ] Section headlines: 58px, Normal weight (400), tracking -2%
- [ ] Body text: 18-22px, Book weight (300)
- [ ] Labels: 14px, weight 600, uppercase, tracking 8%
- [ ] Proper weight hierarchy — lighter for large, heavier for emphasis

## Logo (II Symbol)
- [ ] Built as two vertical bars, not text characters
- [ ] Safespace respected (height of logo on all sides)
- [ ] No effects: no shadow, no stroke, no glow
- [ ] Correct color variant: WHITE on Hero scenes, BLACK/GRAPHITE on Content scenes
- [ ] Not rotated, stretched, or distorted

## Noise
- [ ] Noise overlay applied to ALL scenes (both Hero and Content)
- [ ] Noise opacity: 70%
- [ ] Noise blend mode: overlay
- [ ] Noise is subtle — does not alter saturation, luminosity, or quality

## Blur (Hero Mode Only)
- [ ] Simple blur value: 25 (or Complex: 180 + 0.1 reveal)
- [ ] Background keeps some structure — not cropped into a solid color patch
- [ ] Text and logos remain legible over blurred background
- [ ] No over-blurring that muddies colors

## Layout
- [ ] Generous padding (80px+ on sides)
- [ ] Left-aligned text flow
- [ ] Adequate whitespace — the brand breathes
- [ ] Split compositions where appropriate

## Animation
- [ ] All animations driven by useCurrentFrame()
- [ ] No CSS transitions or animations
- [ ] No Tailwind animate classes
- [ ] Springs use damping: 200 for smooth reveals
- [ ] Elements enter with staggered delays (15 frames apart)
- [ ] Transitions between scenes: fade(), 20 frames

## Graphics
- [ ] Chladni patterns used on title/outro/Hero scenes where appropriate
- [ ] Chladni overlay opacity: 0.03-0.08 (subtle, not dominant)
- [ ] Chladni color appropriate to mode: monochrome for Content, can be over imagery for Hero
- [ ] Voice orbs composited with `mix-blend-mode: screen` (if used)

## Structure
- [ ] Title scene (Hero): II logo + label + title + breadcrumb
- [ ] Outro scene (Hero): II logo + summary + "up next"
- [ ] II watermark on every scene (correct variant per mode)
- [ ] Composition registered in Root.tsx with proper dimensions (1920x1080)

## Overall Feel
- [ ] Minimal and confident
- [ ] Warm but precise
- [ ] Professional — not busy, not flashy
- [ ] Hero scenes feel atmospheric and immersive
- [ ] Content scenes feel clean and instructional
- [ ] Spacious, unhurried pacing
- [ ] Visual rhythm between Hero and Content modes
