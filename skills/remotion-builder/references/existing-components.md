# Existing Components Inventory

Reuse these before creating anything new.

## Scene Components (`src/components/`)

Full-scene compositions — each fills the 1920×1080 frame.

| Component | Layout Type | Props (Schema) | Reusable For |
|-----------|------------|----------------|--------------|
| `WaveformScene` | Split Title Card | `title`, `subtitle` | Any lesson opener |
| `TextRevealScene` | Three-Column Cards | `lines[]`, `sectionLabel`, `headline` | Any 3-concept reveal |
| `OriginStory` | Split Title Card | `label`, `title`, `subtitle` | Any origin/backstory beat |
| `PlatformEvolution` | Three-Step Flow | `label`, `headline` | Any journey/progression |
| `ChannelStrip` | Three-Column Cards | `label`, `headline`, `caption` | Any 3-category showcase |
| `WelcomeCard` | Centered Hero | `title`, `subtitle` | Any big statement moment |
| `ThreePillarsScene` | Three-Column Cards | `sectionLabel`, `headline` | Any 3-item overview |
| `PillarDetailScene` | Split + Feature Grid | `name`, `tagline`, `description`, `features[]`, `isFocus` | Any product deep-dive |
| `FoundationDiagram` | Connected Rows | `label`, `headline`, `bottomText` | Any layered architecture |
| `TimelineProgression` | Split List | `sectionLabel`, `headline`, `subtext` | Any course map or inventory |
| `LearnBuildProve` | Hero Text Panels | `sectionLabel`, `headline` | Any methodology or manifesto |
| `MultiplierVisual` | Scale/Growth Visual | `caption` | Any scale/expansion concept |
| `OutroScene` | Split Title Card | `lessonNumber`, `lessonTitle` | Any lesson closer |

### Additional Scene Components (from overlays/)

| Component | What It Does |
|-----------|-------------|
| `LowerThird` | Lower-third text overlay |
| `CalloutLabel` | Callout/annotation label |
| `SectionBumper` | Section transition bumper |
| `HighlightBox` | Highlight box overlay |

### Additional Scene Components (newer additions)

| Component | What It Does |
|-----------|-------------|
| `AudienceCards` | Audience segment cards |
| `BuyerTypesScene` | Buyer type comparison |
| `DecisionMatrix` | Decision matrix grid |
| `SplitComparison` | Split comparison layout |
| `OneLinerScene` | Single statement scene |
| `TransformationDiagram` | Process transformation diagram |
| `CharacterReveal` | Character/persona reveal |
| `AgentArchitectureScene` | Agent architecture diagram |

## Layout Components (`src/components/layouts/`)

Content-area layouts. Each includes its own AbsoluteFill, NoiseOverlay, and IIWatermark.

| Component | Props | Best For |
|-----------|-------|----------|
| `TwoColumnCards` | `sectionLabel`, `headline`, `card1Title`, `card1Desc`, `card2Title`, `card2Desc` | Comparisons, paired concepts |
| `FourGridCards` | `sectionLabel`, `headline`, 4 cards with `number`, `title`, `description` | 2×2 breakdowns, capability maps |
| `SingleFeatureCard` | `title`, `description` | Deep-dives, spotlight topics |
| `QuoteCard` | `quote`, `attribution` | Mission statements, testimonials |
| `SideBySideComparison` | `sectionLabel`, `headline`, left/right panels with `label`, `title`, `desc` | A vs B, before/after |
| `StatNumber` | `sectionLabel`, `from`, `to`, `unit`, `description` | Single impressive metric |
| `StatRow` | `sectionLabel`, `headline`, 3 stats with `value`, `label` | Multiple metrics |
| `NumberedSteps` | `sectionLabel`, `headline`, steps with `title`, `description` | Instructions, processes |
| `Checklist` | `sectionLabel`, `headline`, items with `text`, `checked` | Requirements, completion |
| `IconRow` | Horizontal row of icon cards with labels | Tech stacks, tool listings |
| `CodeDisplay` | `sectionLabel`, `headline`, code lines | API calls, CLI commands |
| `FullBleedPlaceholder` | `label`, `description` | Screenshot drops, b-roll |
| `SplitQuote` | `quote`, `attribution` | Atmospheric testimonials |

## Shared Elements (`src/components/`)

Used inside scene components — don't pick these directly, but know they exist:

| Component | File | What It Does |
|-----------|------|-------------|
| `ChladniPattern` | `ChladniPattern.tsx` | SVG patterns, 3 variants: organic, geometric, dense |
| `NoiseOverlay` | `ChladniPattern.tsx` | SVG fractal noise grain at 0.04 opacity |
| `AccentLine` | `ChladniPattern.tsx` | Animated horizontal accent line |
| `IILogo` | `IILogo.tsx` | Programmatic II bars (the ElevenLabs icon) |
| `IIWatermark` | `IIWatermark.tsx` | Persistent bottom-right II mark (35% opacity) |

## Foundation Files

| File | What It Does |
|------|-------------|
| `src/brand.ts` | All colors, font family, font sizes, font loading, containerStyle |
| `src/schemas.ts` | Every Zod schema (one file, all components) |
| `src/Root.tsx` | Composition registry (every scene registered here) |
| `src/M01L01v3.tsx` | Combined composition example (13 scenes stitched together) |
| `src/StyleGuide.tsx` | Style guide composition for browsing all 20 layouts |

## When to Create a New Component

**Reuse** when:
- The layout matches an existing component but content is different
- You just need different text, icons, or number of items
- The scene structure (header + content area + watermark) is the same

**Create new** when:
- No existing layout handles the visual concept
- The scene needs fundamentally different structure
- You need interactive/animated elements that don't exist yet

When creating new components, follow the patterns in existing ones:
- Zod schema in `src/schemas.ts` with all fields `.optional()`
- Component typed with `z.infer<typeof Schema>`, defaults in destructuring
- Use `BRAND.*` constants for all colors/fonts
- `spring({ damping: 200 })` for all animations
- Include `NoiseOverlay` + `IIWatermark` in every scene

## V2 Scene Components (`src/components/v2/`)

Full-scene compositions with gradient backgrounds, frosted cards, and entrance/exit animations.

### Shared V2 Components
| Component | File | What It Does |
|-----------|------|-------------|
| `GradientBackground` | `v2/GradientBackground.tsx` | Full-bleed bg image with blur(25px) + noise(7%) + tint(0.12) |
| `BrandedCard` | `v2/BrandedCard.tsx` | Frosted cream glassmorphism card. Props: `children`, `style?`, `borderWidth?` |

### V2 Scene Components
| Component | Layout Type | Mode | Default Background |
|-----------|------------|------|-------------------|
| `WaveformSceneV2` | Full Gradient | Hero | `general/general-bg-01-blue-cyan-chladni.jpg` |
| `TextRevealSceneV2` | Three-Column Frosted Cards | Hero | `chladni-closeup/closeup-03-blue-mountain-orange.jpg` |
| `OriginStoryV2` | Full Gradient | Hero | `chladni-closeup/closeup-07-pink-teal-blend.jpg` |
| `PlatformEvolutionV2` | Three-Step Frosted Flow | Hero | `agents/agents-bg-02-blue-warm.jpg` |
| `ChannelStripV2` | Three-Column Frosted Panels | Hero | `agents/agents-bg-05-grass-warm.jpg` |
| `WelcomeCardV2` | Centered Hero | Hero | `agents/agents-bg-01-green-teal.png` |
| `ThreePillarsSceneV2` | Three-Column Frosted Cards | Hero | `agents/agents-bg-01-blue-green.jpg` |
| `PillarDetailSceneV2` | White/Gradient Split | Hybrid | `gradient/Background6.jpg` |
| `FoundationDiagramV2` | Connected Frosted Rows | Hero | `chladni-closeup/closeup-03-blue-mountain-orange.jpg` |
| `AgentExperienceSceneV2` | Three-Column Frosted Cards | Hero | `agents/agents-bg-02-blue-warm.jpg` |
| `TimelineV4V2` | White/Gradient Split | Hybrid | `gradient/Background11.jpg` |
| `OutroSceneV2` | Full Gradient | Hero | `general/general-bg-03-blue-cyan-clean.jpg` |

All V2 scenes include `ENTER_DELAY = 35` and `exitOpacity` patterns. All accept a `backgroundSrc` prop to override the default background.

### V2 Schemas
Each V2 component has a matching schema in `src/schemas.ts` with the same fields as its V1 counterpart plus `backgroundSrc: z.string().optional()`.
