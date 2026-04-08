---
name: remotion-spec-builder
description: "Build Remotion video spec files — scene-by-scene blueprints using the ElevenLabs two-mode visual system. Creates structured spec documents for any branded video: product demos, training content, marketing videos, internal communications. Use when: 'draft a spec', 'create a video spec', 'spec out the video', 'plan the scenes', 'write a Remotion spec', 'add scenes', or any Remotion composition planning."
---

# ElevenLabs Remotion Spec Drafting Skill (v2)

This skill drafts structured spec files for ElevenLabs branded videos. These specs are the handoff document between content planning and the Remotion code agent that builds the actual video compositions.

## What a Spec File Is

A spec file is a markdown document that describes every scene in a video: what layout it uses, what mode it's in (Hero or Content), what text appears on screen, how long it lasts, and what VO line it accompanies. The Remotion code agent reads this file and generates the React compositions, schemas, and Root.tsx registrations from it.

The spec is NOT code. It's a creative/structural document that maps content to visuals using an established component library.

## How to Use This Skill

### When Drafting a New Spec

1. **Read the brief or script first.** The source document contains the VO, production notes, and objectives. The spec translates that content into scenes.
2. **Load the design reference:** Read `REMOTION_DESIGN_REFERENCE.md` in the project's spec directory for the visual system — colors, typography, animation patterns, scene template diagrams.
3. **Load an example spec** from the project's spec directory to see how a completed spec looks.
4. **Draft the spec** following the template format and save to the project's spec directory as `{ProjectCode}-spec.md`.

### When Updating an Existing Spec

1. Read the existing spec file.
2. Load the design reference if you need to check layout options or component names.
3. Make the changes — add/remove/reorder scenes, update content, adjust durations.
4. Update the Scene Flow Summary table and total duration.

## Two-Mode Scene System

Every scene must be classified as either **Hero** or **Content** mode. This determines the entire visual treatment.

### Mode 1: Hero (Gradient Backgrounds)

Use for: title cards, openers, concept reveals, methodology scenes, outros, atmospheric moments, brand-forward moments.

| Property | Value |
|----------|-------|
| **Background** | Full-bleed image from `brand-assets/backgrounds/` with blur (25px) + noise (7% opacity, overlay blend) + dark tint rgba(0,0,0,0.12) |
| **Text color** | WHITE (#FFFFFF) for headlines and body |
| **Cards** | Dark flat (default): `rgba(0,0,0,0.3)` bg, no blur, border `1px solid rgba(255,255,255,0.12)`, radius 14px, shadow `0 8px 32px rgba(0,0,0,0.3)`. See BrandedCard variant table below. |
| **Card text** | `#FFFFFF` for titles, `rgba(255,255,255,0.65)` for descriptions |
| **II Watermark** | WHITE at 25% opacity |
| **Decorative numbers** | `rgba(30,25,22,0.06)` |
| **Accent lines** | WHITE at 60% opacity |
| **Feel** | Atmospheric, immersive, brand-forward |

### Mode 2: Content (Light Backgrounds)

Use for: step sequences, code examples, detail content, numbered lists, checklists, comparisons, feature grids — anything the viewer needs to read and retain.

| Property | Value |
|----------|-------|
| **Background** | OFF_WHITE (#FAFAFA) solid |
| **Text color** | GRAPHITE (#1E1916) for headlines, DARK_GRAY (#555555) for body |
| **Cards** | Solid CREAM (#F5F3F1) with LIGHT_GRAY (#E0DEDC) borders |
| **Decorative numbers** | LIGHT_GRAY (#E0DEDC) |
| **II Watermark** | GRAPHITE at 35% opacity |
| **Feel** | Clean, readable, instructional |

### Choosing a Mode

- **Hero** when the moment is emotional, transitional, or brand-forward.
- **Content** when the viewer needs to read and retain information. Clarity over atmosphere.
- A typical video alternates: Hero (title) > Content (detail) > Hero (concept reveal) > Content (steps) > Hero (outro).

## Spec File Format

Every spec follows this structure:

```markdown
# {ProjectCode} — [Video Title]

**Project:** [project name or campaign]
**Version:** [number]
**Duration:** ~[total] seconds ([scene count] scenes)
**VO Pace:** ~145 words/min, conversational

## Video Summary
[2-3 sentences: what this video communicates, key takeaway]

## Scene Plan
### Scene 1 — Title Card
[scene block format...]

### Scene N — Outro
[scene block format...]

## Scene Flow Summary
[table of all scenes with mode, layout, duration, component]
```

### Scene Block Format

Each scene follows this exact table + content structure:

```markdown
### Scene [N] — [Scene Name]
| | |
|---|---|
| **Mode** | Hero / Content |
| **Layout** | [from Available Layouts] |
| **Component** | [existing component name, or blank for agent to decide] |
| **Duration** | [seconds] |
| **Background** | [for Hero: background image filename; for Content: omit or "OFF_WHITE"] |
| **VO Line** | [the narration line this scene accompanies] |

**Content:**
- **Section Label:** `[1-3 words, uppercase]`
- **Headline:** `[2-6 words]`
- [additional fields per layout...]

**Notes:** [special behavior, active items, badges, background choice rationale, etc.]
```

## Available Layouts (Quick Reference)

These are the layouts you can assign to scenes. Each has specific content fields and a default mode. Full details with field lists are in the design reference file.

| Layout | Mode | Best For | Example Component |
|--------|------|----------|-------------------|
| **Split Title Card** | Hero | Openers, outros, origin stories | `WaveformScene`, `OutroScene`, `OriginStory` |
| **Three-Column Cards** | Either | 3-item categories, pillars, features | `ThreePillarsScene`, `ChannelStrip`, `TextRevealScene` |
| **Three-Step Flow** | Hero | Journeys, progressions with arrows | `PlatformEvolution` |
| **Split List** | Content | Module maps, numbered inventories | `TimelineProgression` |
| **Centered Hero** | Hero | Welcome moments, big statements | `WelcomeCard` |
| **Connected Rows** | Content | Architecture diagrams, stacked layers | `FoundationDiagram` |
| **Hero Text Panels** | Hero | Methodology, 3 big concepts | `LearnBuildProve` |
| **Split + Feature Grid** | Content | Product deep-dives, 2x2 features | `PillarDetailScene` |
| **Two-Column Cards** | Either | Comparisons, paired concepts | `TwoColumnCards` |
| **Four-Grid Cards** | Content | 2x2 breakdowns, capability maps | `FourGridCards` |
| **Single Feature Card** | Content | Deep-dives, spotlight topics | `SingleFeatureCard` |
| **Quote Card** | Hero | Key quotes, mission statements | `QuoteCard` |
| **Side-by-Side Comparison** | Content | A vs B, before/after | `SideBySideComparison` |
| **Stat Number** | Either | Single impressive metric | `StatNumber` |
| **Stat Row** | Either | Multiple metrics side by side | `StatRow` |
| **Numbered Steps** | Content | Instructions, how-to sequences | `NumberedSteps` |
| **Checklist** | Content | Requirements, completion status | `Checklist` |
| **Code Display** | Content | API calls, CLI commands | `CodeDisplay` |
| **Full-Bleed Placeholder** | Either | Screen recording markers, b-roll markers | `FullBleedPlaceholder` |
| **Split Quote** | Hero | Atmospheric testimonials | `SplitQuote` |

### New Layouts (v2 — from pitch decks)

| Layout | Mode | Best For | Example Component |
|--------|------|----------|-------------------|
| **Agenda Grid** | Hero | Table of contents, numbered section list | `AgendaScene` |
| **Credibility Wall** | Hero | Stats mosaic + logo wall (40M users, $11B...) | `CredibilityWallScene` |
| **Conversation Transcript** | Hero | Chat bubble dialog with emotion tags | `ConversationTranscriptScene` |
| **Voice Qualities Radial** | Hero | Radial word cloud of voice attributes | `VoiceQualitiesScene` |
| **Integration Map** | Hero | Categorized logo columns (CRM, Telephony...) | `IntegrationMapScene` |
| **Use Case Card** | Hero | Standardized use case: title, benefit, impact | `UseCaseCardScene` |
| **Deployment Roadmap** | Hero | 4 numbered vertical cards in a row | `DeploymentRoadmapScene` |
| **Compliance Badges** | Hero | Security cert badge grid (SOC2, ISO...) | `ComplianceBadgeScene` |
| **Enterprise Function Grid** | Hero | 5-column business function breakdown | `EnterpriseFunctionGridScene` |
| **End-to-End Pipeline** | Hero | Horizontal STT→VAD→LLM→TTS flow | `EndToEndPipelineScene` |
| **Comparison Table** | Either | Multi-column feature comparison with checks | `ComparisonTableScene` |
| **Six Pillar Grid** | Hero | 2x3 image+title grid | `SixPillarScene` |
| **Two Screenshot** | Either | Side-by-side screenshot showcase | `TwoScreenshotScene` |
| **VS Comparison** | Hero | Head-to-head with "vs" divider | `VSComparisonScene` |

The **Mode** column shows the default/recommended mode. Layouts marked **Either** work in both modes — choose based on the scene's emotional weight. Override the default only when it serves the content.

**Reuse existing components with different content before suggesting new ones.** A new component should only be created if no existing layout handles the visual concept.

## BrandedCard Variants

The `BrandedCard` component (used for all cards on Hero scenes) has 8 variants. Choose based on the visual density and desired feel:

| Variant | Background | Blur | Border | Text | Use |
|---------|-----------|------|--------|------|-----|
| `darkflat` (DEFAULT) | `rgba(0,0,0,0.3)` | none | `1px solid rgba(255,255,255,0.12)` | White | Default for all Hero mode cards. No blur artifacts in dense grids. |
| `darkglass` | `rgba(0,0,0,0.3)` | `blur(20px)` | `1px solid rgba(255,255,255,0.12)` | White | Same as darkflat but with backdrop blur. Use for isolated cards only. |
| `glass` | `rgba(255,255,255,0.08)` | `blur(40px)` | `1px solid rgba(255,255,255,0.2)` | White | Deep glassmorphism. Background bleeds through. |
| `baseline` | `rgba(245,243,241,0.88)` | `blur(24px)` | `1px solid rgba(255,255,255,0.12)` | Dark | Frosted cream. Original v2 style. |
| `outline` | transparent | none | `1px solid rgba(255,255,255,0.25)` | White | No fill, border only. |
| `pill` | `rgba(255,255,255,0.12)` | `blur(30px)` | `1px solid rgba(255,255,255,0.18)` | White | Large 24px radius. |
| `acrylic` | `rgba(245,243,241,0.45)` | `blur(12px)` | `1px solid rgba(255,255,255,0.15)` | Dark | Fluent-inspired with noise texture. |
| `gradientborder` | `rgba(0,0,0,0.15)` | `blur(20px)` | gradient stroke | White | Shimmer gradient border. |

**Key rule:** For grids of 4+ cards, always use `darkflat` — `backdrop-filter` causes rendering artifacts when multiple blurred elements overlap.

## Design System Essentials

These are the core visual rules. The code agent enforces them, but knowing them helps you make good spec decisions.

### Colors

**Brand palette:**
| Name | Hex | Use |
|------|-----|-----|
| GRAPHITE | #1E1916 | Text on Content scenes. Never backgrounds. |
| OFF_WHITE | #FAFAFA | Content scene backgrounds |
| CREAM | #F5F3F1 | Card fills on Content scenes |
| LIGHT_GRAY | #E0DEDC | Dividers, borders, decorative numbers (Content) |
| MID_GRAY | #999999 | Labels, secondary text, section labels (Content) |
| DARK_GRAY | #555555 | Body text, descriptions (Content) |
| WHITE | #FFFFFF | Text on Hero scenes, Hero watermarks |
| BLACK | #000000 | Pure black (use sparingly) |

**Key rule:** Color comes from images, not palettes. Hero scenes get their color from background imagery. Content scenes stay monochrome. No gray shades mixed with color accents.

### Typography

- **KMR Waldenburg** only. Two weights: Book (300, refined) and Normal (400, confident).
- Large text (titles, headlines): Book weight 300
- Small text (labels, tags): weight 600, uppercase, loose tracking
- Title card headlines: 78px. Scene headlines: 52px. Card names: 34-38px. Descriptions: 20-24px. Labels: 16px.
- **Never bold (700) for headlines.** Light weight for large text, heavier for small text.

### Noise Treatment

- **Apply noise to all scenes** — both Hero and Content modes.
- Settings: 7% opacity, overlay blend mode, SVG fractal noise.
- Must be delicate — should not alter saturation or quality.

### Blur (Hero Mode)

- Background images are blurred at 25px + dark tint rgba(0,0,0,0.12) before content is composited.
- Avoid tight crops that become flat color patches. Keep some texture/structure in the background.

### Animation

- All springs use `damping: 200` — snappy, no bounce
- Cards enter from below (translateY 40px > 0), staggered 8-20 frames apart
- v2 transitions: fade only, 45 frames, linear timing
- v2 scenes have `ENTER_DELAY = 35` so content enters after the crossfade overlap
- II watermark on every scene (WHITE for Hero, GRAPHITE for Content)

### Scene Flow Rules

- **Title card is always Scene 1** — Split Title Card layout, Hero mode
- **Outro is always the last scene** — Split Title Card layout, Hero mode
- Hero and Content scenes should alternate where natural — creates visual rhythm
- When a video has screen recordings, use `FullBleedPlaceholder` to mark where footage gets swapped in during post-production. Use hard cuts (no transitions) around placeholder scenes.
- Motion scenes adjacent to placeholders get extra duration (2-4s) for trim room in editing

## New Components (v2)

These components are new in v2 and available for the code agent to build:

- **`GradientBackground`** — Applies a background image from `brand-assets/backgrounds/` with blur (25px) + noise overlay (7%, overlay blend) + dark tint. Props: `src`, `blur` (default 25), `noiseOpacity` (default 0.07), `tint` (default 0.12). Used on all Hero mode scenes.
- **`BrandedCard`** — Multi-variant card component with `variant` prop (see variant table above). Default variant is `darkflat`. Helper `getCardColors(variant)` returns matching text colors (title, description, label, divider, decorativeNumber).
- **`VoiceOrb`** — Animated metallic orb accent from `brand-assets/voice-orbs/`. Composited with `mix-blend-mode: screen` to remove the black background. Props: `color`, `size`, `opacity`, `animate`. Use for atmospheric accents on Hero scenes.
- **`IIWatermark`** — Updated with `color` prop: `"white"` (25% opacity, Hero) or `"graphite"` (35% opacity, Content).

## Content Guidelines for Specs

### Text on Screen

- **Headlines:** 2-6 words. Short and punchy.
- **Section labels:** 1-3 words, uppercase. Category, not sentence.
- **Card descriptions:** 1-2 sentences max. These appear on screen — they're not VO scripts.
- **Use `\n`** for explicit line breaks in titles/subtitles.

### VO Pacing

- ~145 words per minute at conversational pace
- 3 min video = ~435 words of VO, ~10-14 scenes
- 4 min video = ~580 words of VO, ~12-16 scenes
- Each scene's duration should match the VO line it accompanies

### Scene Planning

- **10-16 scenes is typical** for a video in the 3-4 minute range
- Minimum scene duration: 8 seconds (to allow entrance + hold + exit animations)
- When a video includes screen recordings, use `FullBleedPlaceholder` to mark the swap-in points

### Background Selection for Hero Scenes

When specifying a background image for a Hero scene, choose from:

- **`backgrounds/agents/`** — 9 images. Green, teal, blue tones. Best for: agent-related content, platform scenes, tech moments.
- **`backgrounds/chladni-closeup/`** — 15 images. Diverse natural photography with blur. Best for: concept reveals, emotional moments, methodology scenes.
- **`backgrounds/general/`** — 7 images. Blue/cyan/teal gradients with Chladni overlays. Best for: title cards, generic hero moments.
- **`backgrounds/gradient/`** — 16 atmospheric gradients (Background0–15.jpg). Clean, versatile. Great default choice for any Hero scene.
- **`backgrounds/creative/`** — 1 image. Coral sunset. Best for: creative/warm moments.

Include the filename in the scene block (e.g., `**Background:** agents-bg-01-blue-green.jpg`). The code agent handles the compositing pipeline (image > blur > noise > content).

## Scene Mode Classification

Quick reference for which mode each existing component uses:

**Hero mode scenes:**
`WaveformScene`, `TextRevealScene`, `OriginStory`, `LearnBuildProve`, `OutroScene`, `WelcomeCard`, `PlatformEvolution`, `ThreePillarsScene`, `AgendaScene`, `CredibilityWallScene`, `ConversationTranscriptScene`, `VoiceQualitiesScene`, `IntegrationMapScene`, `UseCaseCardScene`, `DeploymentRoadmapScene`, `ComplianceBadgeScene`, `EnterpriseFunctionGridScene`, `EndToEndPipelineScene`, `SixPillarScene`, `VSComparisonScene`

**Content mode scenes:**
`TimelineProgression`, `CodeDisplay`, `NumberedSteps`, `Checklist`, `SingleFeatureCard`, `SideBySideComparison`, `FoundationDiagram`, `PillarDetailScene`, `FourGridCards`

**Either mode (context-dependent):**
`ChannelStrip`, `TwoColumnCards`, `StatNumber`, `StatRow`, `QuoteCard`, `SplitQuote`, `FullBleedPlaceholder`, `ComparisonTableScene`, `TwoScreenshotScene`

## Scene Flow Summary Table

The Scene Flow Summary at the end of the spec should use this format:

```
| # | Scene           | Mode    | Layout            | Duration | Component         |
|---|-----------------|---------|-------------------|----------|-------------------|
| 1 | Title Card      | Hero    | Split Title Card  | 8s       | WaveformScene     |
| 2 | Architecture    | Content | Connected Rows    | 12s      | FoundationDiagram |
| 3 | Concept Reveal  | Hero    | Centered Hero     | 10s      | WelcomeCard       |
| 4 | Steps           | Content | Numbered Steps    | 12s      | NumberedSteps     |
| 5 | Outro           | Hero    | Split Title Card  | 10s      | OutroScene        |
```

## File Naming

Spec files: `{ProjectCode}-spec.md`

Examples: `M01L01v3-spec.md`, `Q2-product-demo-v1-spec.md`, `onboarding-overview-v2-spec.md`

Save all specs to the project's designated spec directory.

## Brand Assets

All brand assets are in `public/brand-assets/` within the Remotion project:

- **Backgrounds** — `backgrounds/agents/` (9), `backgrounds/chladni-closeup/` (15), `backgrounds/general/` (7), `backgrounds/gradient/` (16), `backgrounds/creative/` (1)
- **Voice Orbs** — `voice-orbs/` — 8 metallic orbs on black (teal, purple-blue, pink, coral, green, gold, hotpink, lavender). Use `mix-blend-mode: screen`.
- **Icons** — `icons/cream/` (~130), `icons/black/` (~130), `icons/white/` (~130), `icons/product/` (~10)
- **Shared textures** — `noise-texture.jpg`, `seamless-texture.jpg`
- **Color tokens** — `color-tokens.json`
- **Brand guidelines** — `BRAND_GUIDELINES.md`

## Reference Files

Load these from the project's spec directory when you need full details:

- **`REMOTION_DESIGN_REFERENCE.md`** — The complete visual system: ASCII layout diagrams, exact pixel values, animation sequences, asset locations, delivery checklist. Load this when you need precise visual details.
- **`LESSON_SPEC_TEMPLATE.md`** (if present) — Blank template with all layout field definitions, established components tables, and instructions for the build agent. Load when drafting a new spec.

## Remotion Technical Rules (for reference)

The `rules/` directory contains detailed Remotion technical documentation. These are for understanding how the code agent works — you don't need them for spec drafting, but they can help you understand constraints:

- `rules/animations.md` — useCurrentFrame, interpolate
- `rules/timing.md` — spring configs, easing
- `rules/transitions.md` — TransitionSeries, fade, slide
- `rules/sequencing.md` — Sequence, Series, delay
- `rules/compositions.md` — Composition setup
- `rules/audio.md` — Audio import, volume
- `rules/voiceover.md` — ElevenLabs TTS integration

Load any of these if you need technical context for a spec decision (e.g., "can we do X in Remotion?")
