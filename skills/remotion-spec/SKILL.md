---
name: elevenlabs-remotion
description: "Draft and update Remotion video spec files for ElevenLabs Academy lesson videos. This skill creates structured spec documents that get handed to a Remotion code agent for implementation. Use this skill whenever Jake asks to: draft a spec, create a spec, write a video spec, plan a lesson video, spec out a lesson, update a spec, add scenes to a spec, or any variation of creating/editing the structured spec files that live in `10 - Remotion/`. Also trigger when working on Remotion composition planning, scene breakdowns, or visual planning for any ElevenLabs Academy lesson. Even casual requests like 'spec out module 3 lesson 1' or 'plan the video for M5-L2' or 'what scenes would we need for the knowledge base lesson' should use this skill."
---

# ElevenLabs Remotion Spec Drafting Skill (v2)

This skill drafts structured spec files for ElevenLabs Academy lesson videos. These specs are the handoff document between content planning (Jake) and the Remotion code agent that builds the actual video compositions.

## What Changed in v2

v2 introduces a **two-mode scene system** based on the official 2025 Brand Design System. Every scene is now classified as either **Hero** or **Content** mode, which determines its background treatment, text color, card style, and watermark variant. This replaces the v1 all-light monochrome approach with a system that matches the full brand guidelines.

## What a Spec File Is

A spec file is a markdown document that describes every scene in a lesson video: what layout it uses, what mode it's in (Hero or Content), what text appears on screen, how long it lasts, and what VO beat it accompanies. The Remotion code agent reads this file and generates the React compositions, schemas, and Root.tsx registrations from it.

The spec is NOT code. It's a creative/structural document that maps content to visuals using an established component library.

## How to Use This Skill

### When Drafting a New Spec

1. **Read the lesson script first.** The script (in `01 - Certification Program/Lessons/`) contains the VO, production notes, and learning objectives. The spec translates that content into scenes.
2. **Load the template:** Read `10 - Remotion/LESSON_SPEC_TEMPLATE.md` for the exact format, available layouts, and established components.
3. **Load the design reference:** Read `10 - Remotion/REMOTION_DESIGN_REFERENCE.md` for the visual system — colors, typography, animation patterns, scene template diagrams.
4. **Load the example:** Read `10 - Remotion/M01L01v3-spec.md` to see how a completed spec looks.
5. **Draft the spec** following the template format and save to `10 - Remotion/M{XX}L{YY}v{Z}-spec.md`.

### When Updating an Existing Spec

1. Read the existing spec file.
2. Load the template and design reference if you need to check layout options or component names.
3. Make the changes — add/remove/reorder scenes, update content, adjust durations.
4. Update the Scene Flow Summary table and total duration.

## Two-Mode Scene System

Every scene must be classified as either **Hero** or **Content** mode. This determines the entire visual treatment.

### Mode 1: Hero (Gradient Backgrounds)

Use for: title cards, module openers, concept reveals, methodology scenes, outros, atmospheric moments.

| Property | Value |
|----------|-------|
| **Background** | Full-bleed image from `brand-assets/backgrounds/` with blur (25px) + noise (70% opacity, overlay blend) + optional Chladni overlay |
| **Text color** | WHITE (#FFFFFF) for headlines and body |
| **Cards** | Frosted cream: `rgba(245, 243, 241, 0.88)`, `backdrop-filter: blur(24px)`, border `1px solid rgba(255,255,255,0.12)`, radius 14px, shadow `0 8px 32px rgba(0,0,0,0.12)` |
| **II Watermark** | WHITE at 25% opacity |
| **Feel** | Atmospheric, immersive, brand-forward |

### Mode 2: Content (Light Backgrounds)

Use for: step sequences, code examples, detail content, numbered lists, checklists, comparisons, feature grids.

| Property | Value |
|----------|-------|
| **Background** | OFF_WHITE (#FAFAFA) solid |
| **Text color** | GRAPHITE (#1E1916) for headlines, DARK_GRAY (#555555) for body |
| **Cards** | Solid CREAM (#F5F3F1) with LIGHT_GRAY (#E0DEDC) borders |
| **II Watermark** | GRAPHITE at 35% opacity |
| **Feel** | Clean, readable, instructional |

### Choosing a Mode

- **Hero** when the moment is emotional, transitional, or brand-forward. The learner should feel something.
- **Content** when the learner needs to read and retain information. Clarity over atmosphere.
- A typical lesson alternates: Hero (title) > Content (detail) > Hero (concept reveal) > Content (steps) > Hero (outro).

## Spec File Format

Every spec follows this structure:

```markdown
# M{XX}L{YY}v{Z} — [Lesson Title]

**Module:** [number] — [module name]
**Lesson:** [number] — [lesson name]
**Version:** [number]
**Duration:** ~[total] seconds ([scene count] scenes)
**VO Pace:** ~145 words/min, conversational

## Video Summary
[2-3 sentences: what this lesson teaches, key takeaway]

## Scene Plan
### Scene 1 — Title Card
[scene block format...]

### Scene N — Outro
[scene block format...]

## Scene Flow Summary
[table of all scenes with mode, layout, duration, component, screengrab]
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
| **VO Beat** | [the VO line this scene accompanies] |
| **Screengrab** | [Yes / No] |

**Content:**
- **Section Label:** `[1-3 words, uppercase]`
- **Headline:** `[2-6 words]`
- [additional fields per layout...]

**Notes:** [special behavior, active items, badges, background choice rationale, etc.]
```

## Available Layouts (Quick Reference)

These are the layouts you can assign to scenes. Each has specific content fields and a default mode. Full details with field lists are in the template file.

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
| **Full-Bleed Placeholder** | Either | Screenshot drops, b-roll markers | `FullBleedPlaceholder` |
| **Split Quote** | Hero | Atmospheric testimonials | `SplitQuote` |

### New Layouts (v2 — from pitch decks)

| Layout | Mode | Best For | Example Component |
|--------|------|----------|-------------------|
| **Agenda Grid** | Hero | Module table of contents, numbered section list | `AgendaScene` |
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

## Design System Essentials

These are the core visual rules. The code agent enforces them, but knowing them helps you make good spec decisions.

### Colors

**Brand palette:**
| Name | Hex | Use |
|------|-----|-----|
| GRAPHITE | #1E1916 | Text on Content scenes. Never backgrounds. |
| OFF_WHITE | #FAFAFA | Content scene backgrounds |
| CREAM | #F5F3F1 | Card fills (solid on Content, frosted on Hero) |
| LIGHT_GRAY | #E0DEDC | Dividers, borders, decorative numbers |
| MID_GRAY | #999999 | Labels, secondary text, section labels |
| DARK_GRAY | #555555 | Body text, descriptions |
| WHITE | #FFFFFF | Text on Hero scenes, Hero watermarks |
| BLACK | #000000 | Pure black (use sparingly) |

**Key rule:** Color comes from images, not palettes. Hero scenes get their color from background imagery. Content scenes stay monochrome. No gray shades mixed with color accents.

### Typography
- **KMR Waldenburg** only. Two weights: Normal (400, confident) and Book (300, refined).
- Large text (titles, headlines): Book weight 300, tight tracking
- Small text (labels, tags): weight 600, uppercase, loose tracking
- Scene titles: 44-68px. Card names: 28px. Descriptions: 16-17px. Labels: 14px.

### Noise Treatment
- **Apply noise to all scenes** — both Hero and Content modes.
- Settings: 70% opacity, overlay blend mode.
- Must be delicate — should not alter saturation, luminosity, or quality.

### Blur (Hero Mode)
- **Simple blur:** value 25. Softens background images into atmosphere.
- **Complex blur:** Layer 1 blur 180 over composition, Layer 2 at opacity 0.1 revealing parts below. Base image stays unedited.
- Avoid tight crops that become color patches. Keep some structure.

### Animation
- All springs use `damping: 200` — snappy, no bounce
- Cards enter from below (translateY 40px > 0), staggered 15 frames apart
- Transitions: fade only, 20 frames, between every scene
- II watermark on every scene (WHITE for Hero, GRAPHITE for Content)

### Scene Flow Rules
- Every lesson: Title Card (Hero) > content scenes > Outro (Hero)
- Hero and Content scenes should alternate where natural — creates visual rhythm
- "ElevenLabs" is one word, capital E and L

## New Components (v2)

These components are new in v2 and available for the code agent to build:

- **`GradientBackground`** — Applies a background image from `brand-assets/backgrounds/` with blur (25px) + noise overlay (70%). Used on all Hero mode scenes.
- **`BrandedCard`** — Frosted cream card for Hero scenes. `rgba(245, 243, 241, 0.88)` background, `backdrop-filter: blur(24px)`, border `1px solid rgba(255,255,255,0.12)`, radius 14px.
- **`VoiceOrb`** — Animated metallic orb accent from `brand-assets/voice-orbs/`. Composited with `mix-blend-mode: screen` to remove the black background. Use for atmospheric accents on Hero scenes.
- **`IIWatermark`** — Updated with a `variant` prop: `"dark"` (GRAPHITE, 35% opacity) for Content scenes, `"light"` (WHITE, 25% opacity) for Hero scenes.

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
- Each scene's duration should match the VO beat it accompanies

### Scene Planning
- **Title card is always Scene 1** — Split Title Card layout, `WaveformScene` component, **Hero mode**, background from `backgrounds/agents/` or `backgrounds/chladni-closeup/`
- **Outro is always the last scene** — Split Title Card layout, `OutroScene` component, **Hero mode**, points to NEXT lesson
- **10-16 scenes is typical** for a lesson video
- When a lesson has screen recordings, use `FullBleedPlaceholder` to mark where footage gets swapped in during post-production

### Background Selection for Hero Scenes
When specifying a background image for a Hero scene, choose from:
- **`backgrounds/agents/`** — 9 images. Green, teal, blue tones. Best for: agent-related content, platform scenes, tech moments.
- **`backgrounds/chladni-closeup/`** — 15 images. Diverse natural photography with blur. Best for: concept reveals, emotional moments, methodology scenes.
- **`backgrounds/general/`** — 7 images. Blue/cyan/teal gradients, some with Chladni overlays. Best for: title cards, generic hero moments.
- **`backgrounds/creative/`** — 1 image. Coral sunset. Best for: creative/warm moments.

Include the filename in the scene block (e.g., `**Background:** agents-bg-01-blue-green.jpg`). The code agent handles the compositing pipeline (image > blur > noise > content).

### Text Companion Screengrabs
Every spec must include a **Screengrab** field on each scene (Yes/No). Scenes marked Yes get a static PNG frame exported at their fully-rendered state (all animations complete, all elements visible). These stills become the annotated screenshots and reference cards in the lesson's text companion (built in WorkRamp).

**Mark Yes for:** architecture diagrams, feature grids, comparison cards, step flows, checklists, numbered steps, any visual the learner would want to reference during hands-on practice.

**Mark No for:** title cards, outros, atmospheric/transitional scenes (logo reveals, multiplier visuals), scenes only meaningful in motion.

The Scene Flow Summary table must include Mode and Screengrab columns:
```
| # | Scene           | Mode    | Layout            | Duration | Component         | Screengrab |
|---|-----------------|---------|-------------------|----------|-------------------|------------|
| 1 | Title Card      | Hero    | Split Title Card  | 8s       | WaveformScene     | No         |
| 2 | Architecture    | Content | Connected Rows    | 12s      | FoundationDiagram | Yes        |
| 3 | Concept Reveal  | Hero    | Centered Hero     | 10s      | WelcomeCard       | No         |
| 4 | Steps           | Content | Numbered Steps    | 12s      | NumberedSteps     | Yes        |
| 5 | Outro           | Hero    | Split Title Card  | 10s      | OutroScene        | No         |
```

Output path: `10 - Remotion/screengrabs/M{XX}L{YY}/scene-{N}-{scene-name-slug}.png` (1920x1080).

See the full screengrab spec in `LESSON_SPEC_TEMPLATE.md` under "Text Companion Screengrabs."

### Content Model — Not Every Lesson Gets a Video

Not every lesson needs a Remotion video. The content model is:

- **Text companion:** Every lesson gets one (baseline).
- **Video (Remotion + screen recordings):** When the lesson needs audio examples, motion graphics for concepts, or a "wow moment" (e.g., first agent conversation). Video is the special occasion, not the default.
- **Arcade interactive demo:** When the lesson is primarily UI navigation or configuration — clickable walkthroughs the learner steps through at their own pace. Recorded via Arcade's Mac app (captures browser, terminal, any screen).
- **Some lessons get both video AND Arcade** — video for the concept piece, Arcade for the hands-on walkthrough.

When drafting a spec, check whether the lesson is designated for video, Arcade, or both. If a lesson is Arcade-only, no Remotion spec is needed. The lesson script and curriculum walkthrough indicate which visual medium each lesson uses.

## Scene Mode Classification

Quick reference for which mode each existing component uses:

**Hero mode scenes:**
`WaveformScene`, `TextRevealScene`, `OriginStory`, `LearnBuildProve`, `OutroScene`, `WelcomeCard`, `PlatformEvolution`, `ThreePillarsScene`, `AgendaScene`, `CredibilityWallScene`, `ConversationTranscriptScene`, `VoiceQualitiesScene`, `IntegrationMapScene`, `UseCaseCardScene`, `DeploymentRoadmapScene`, `ComplianceBadgeScene`, `EnterpriseFunctionGridScene`, `EndToEndPipelineScene`, `SixPillarScene`, `VSComparisonScene`

**Content mode scenes:**
`TimelineProgression`, `CodeDisplay`, `NumberedSteps`, `Checklist`, `SingleFeatureCard`, `SideBySideComparison`, `FoundationDiagram`, `PillarDetailScene`, `FourGridCards`

**Either mode (context-dependent):**
`ChannelStrip`, `TwoColumnCards`, `StatNumber`, `StatRow`, `QuoteCard`, `SplitQuote`, `FullBleedPlaceholder`, `ComparisonTableScene`, `TwoScreenshotScene`

## Project Context

### Module Structure
8 training modules + separate certification exam:
1. The Platform & Your First Agent
2. Voice & AI Foundations — How Your Agent Works
3. Prompt Engineering — LLM Configuration & Conversation Design
4. Knowledge & Tools — Connect Your Agent to the World
5. Advanced Architecture — Workflows, API & Multi-Agent Systems
6. Deployment — Phone, Web, Mobile & Enterprise Channels
7. Testing & Evaluation — Scenarios, Experiments & CI/CD
8. Production Operations — Analytics, Cost, Privacy & Scale
- Certification Exam — Build an agent end-to-end for a defined use case (separate, paywalled)

### Credential Name
"ElevenLabs Certified ElevenAgent Builder"

### Voice-First but Omnichannel
Lead with voice, acknowledge chat and messaging channels. Don't feed the "we only do voice" perception gap.

## File Naming
Spec files: `M{XX}L{YY}v{Z}-spec.md`
Examples: `M01L01v3-spec.md`, `M03L02v1-spec.md`, `M07L04v1-spec.md`

Save all specs to `10 - Remotion/` in the workspace.

## Reference Files

Load these from `10 - Remotion/` in the workspace when you need full details:

- **`LESSON_SPEC_TEMPLATE.md`** — The blank template with all layout field definitions, established components tables, screengrab instructions, and instructions for the build agent. Load this when drafting a new spec.
- **`REMOTION_DESIGN_REFERENCE.md`** — The complete visual system: ASCII layout diagrams, exact pixel values, animation sequences, asset locations, delivery checklist. Load this when you need precise visual details.
- **`M01L01v3-spec.md`** — The completed spec for Lesson 1.1. Load this as a reference example when drafting new specs.

## Brand Assets

### Bundled with This Skill (`assets/`)
- `assets/logos/` — Full wordmark PNGs + SVGs (black and white)
- `assets/ii-icon/` — II symbol PNGs + SVGs (black and white)
- `assets/fonts/` — KMR Waldenburg OTFs (Buch, Normal, Halbfett, Fett)
- `assets/covers/` — Chladni pattern cover images (JPG)
- `assets/gradients/` — Atmospheric gradient backgrounds (JPG)

### Expanded Inventory in Remotion Project (`public/brand-assets/`)
The Remotion project at `/Users/jakerains/Projects/remotion/` has a full brand assets library:
- **34 background images** in `backgrounds/` (agents/ 9, chladni-closeup/ 15, general/ 7, creative/ 1, + 2 shared textures)
- **8 voice orbs** in `voice-orbs/` (teal, purple-blue, pink, coral, green, gold, hotpink, lavender on black)
- **400+ icons** in `icons/` (cream/black/white 130 each, product/ 10)
- **1 architecture diagram** in `diagrams/`
- **Color tokens** in `color-tokens.json`
- **Brand guidelines** in `BRAND_GUIDELINES.md`

Load `references/bundled-assets.md` for the full inventory, `staticFile()` usage examples, and the background compositing pipeline.

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
