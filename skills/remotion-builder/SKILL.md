---
name: remotion-builder
description: "[DEPRECATED — use hyperframes-builder instead] Build Remotion video compositions from spec files. The Remotion video pipeline has been retired in favor of HyperFrames; use hyperframes-builder for new work. This skill is preserved for reference and for maintaining legacy Remotion code. To re-implement an existing Remotion video in HyperFrames, hand the original spec to hyperframes-builder — the spec format is implementation-agnostic."
---

> **DEPRECATED**: This skill builds Remotion (React/TypeScript) code from specs. The Remotion-based ElevenLabs video pipeline has been retired in favor of HyperFrames (HTML+GSAP). For new work, use **`hyperframes-builder`** which produces HyperFrames compositions from the same spec format.
>
> This skill remains in the plugin only as a historical reference. It will be removed in a future release.

---

# ElevenLabs Remotion Builder Skill

This skill takes spec files and builds actual Remotion compositions for ElevenLabs branded videos. It's the implementation counterpart to the `remotion-spec-builder` spec-drafting skill.

## Your Role

You are the code agent. You receive a completed spec file (a markdown document describing every scene in a video) and you produce:

1. **Scene components** — React/TSX files in `src/components/`
2. **Zod schemas** — added to `src/schemas.ts`
3. **Combined composition** — a file in `src/` stitching all scenes with TransitionSeries
4. **Root.tsx registrations** — individual Composition entries for each scene + the combined preview

## Project Setup

This is an existing Remotion 4.0 project. Key facts:

- **Remotion version:** 4.0.438
- **React:** 19
- **Zod:** 4.3.6
- **FPS:** 30
- **Resolution:** 1920×1080

### File Structure

```
remotion-project/
├── public/
│   ├── fonts/              — KMR Waldenburg OTFs (300, 400, 600, 700)
│   ├── logos/              — Logo-black/white PNGs, II icon SVGs, platform banners
│   ├── covers/             — Chladni pattern cover images
│   ├── icons/              — Brand icons
│   └── brand-assets/       — Full brand asset library (v2)
│       ├── backgrounds/    — agents/ (9), chladni-closeup/ (15), general/ (7), gradient/ (16), creative/ (1)
│       ├── voice-orbs/     — 8 metallic orbs on black
│       ├── icons/          — cream/ black/ white/ product/
│       ├── diagrams/       — Platform architecture diagram
│       └── ui-highlights/  — Phone agent screenshot
├── src/
│   ├── brand.ts            — Colors, typography, font loading (BRAND constants)
│   ├── schemas.ts          — All Zod schemas (one file, all components)
│   ├── Root.tsx            — Composition registry
│   ├── [CompositionName].tsx  — Combined composition files (one per video project)
│   ├── components/
│   │   ├── ChladniPattern.tsx  — SVG patterns + NoiseOverlay + AccentLine
│   │   ├── IILogo.tsx          — Programmatic II bars
│   │   ├── IIWatermark.tsx     — Persistent bottom-right mark
│   │   ├── WaveformScene.tsx   — Split title card (opener)
│   │   ├── OutroScene.tsx      — Split title card (closer)
│   │   ├── [other scene components...]
│   │   ├── v2/                     — V2 Hero mode components
│   │   │   ├── GradientBackground.tsx   — Full-bleed bg with blur+noise+tint
│   │   │   ├── BrandedCard.tsx          — Card with variant system (darkflat default). Exports: BrandedCard, getCardColors(), CardVariant type
│   │   │   ├── WaveformSceneV2.tsx      — Full gradient title card
│   │   │   ├── TextRevealSceneV2.tsx    — Gradient + frosted 3-card reveal
│   │   │   ├── OriginStoryV2.tsx        — Full gradient origin narrative
│   │   │   ├── PlatformEvolutionV2.tsx  — Gradient + frosted step flow
│   │   │   ├── ChannelStripV2.tsx       — Gradient + frosted channel panels
│   │   │   ├── WelcomeCardV2.tsx        — Gradient centered hero
│   │   │   ├── ThreePillarsSceneV2.tsx  — Gradient + frosted platform cards
│   │   │   ├── PillarDetailSceneV2.tsx  — White/gradient split + 2x2 grid
│   │   │   ├── FoundationDiagramV2.tsx  — Gradient + frosted connected rows
│   │   │   ├── AgentExperienceSceneV2.tsx — Gradient + frosted 3-card showcase
│   │   │   ├── TimelineV4V2.tsx         — White/gradient split + module grid
│   │   │   └── OutroSceneV2.tsx         — Full gradient outro
│   │   └── layouts/
│   │       ├── TwoColumnCards.tsx
│   │       ├── FourGridCards.tsx
│   │       ├── SingleFeatureCard.tsx
│   │       ├── QuoteCard.tsx
│   │       ├── SideBySideComparison.tsx
│   │       ├── StatNumber.tsx
│   │       ├── StatRow.tsx
│   │       ├── NumberedSteps.tsx
│   │       ├── Checklist.tsx
│   │       ├── IconRow.tsx
│   │       ├── CodeDisplay.tsx
│   │       ├── FullBleedPlaceholder.tsx
│   │       └── SplitQuote.tsx
│   └── overlays/
│       ├── LowerThird.tsx
│       ├── CalloutLabel.tsx
│       ├── SectionBumper.tsx
│       └── HighlightBox.tsx
└── package.json
```

## How to Use This Skill

### Step 0: Load Remotion Best Practices

Before building anything, invoke `/remotion-best-practices` to load the full Remotion domain knowledge — animations, timing, transitions, audio, text effects, sequencing, 3D, light leaks, captions, and more. The rules in this skill cover ElevenLabs-specific brand patterns; best practices covers the broader Remotion API that you'll need for implementation. Always load both.

### Step 1: Read the Spec

The spec file tells you everything: what scenes to build, what layout each uses, what content goes where, and how long each scene lasts. Read it carefully — it's your blueprint.

### Step 2: Check What Already Exists

Before writing any code, check what components already exist. Most specs reuse existing layouts with different content — you shouldn't create a new component unless the spec explicitly calls for a new layout type.

Load `references/existing-components.md` for the full inventory of scene components, layout components, and shared elements.

### Step 3: Build

For each scene in the spec:

1. **If the component exists** — use it with props from the spec's Content section
2. **If a layout exists but no scene component** — wrap the layout in a scene shell (header + background + watermark)
3. **If nothing fits** — create a new component following the established patterns

Then:
4. Add any new schemas to `src/schemas.ts`
5. Create the combined composition file in `src/`
6. Register everything in `src/Root.tsx`

### Step 4: Verify

After building, run `npx remotion studio` to verify. Check that:
- All scenes render without errors
- Transitions are smooth (fade, 45 frames for v2)
- Typography and colors match the brand
- II watermark appears on all scenes
- Duration matches the spec

## Core Rules

Load `rules/` files for detailed patterns. Here's the essential summary:

### Brand System (always enforced)

Load `rules/brand-system.md` for the complete reference. Key constants:

```typescript
import { BRAND } from "../brand";

// Colors — monochrome only
BRAND.GRAPHITE    // #1E1916 — text/elements, NEVER backgrounds
BRAND.OFF_WHITE   // #FAFAFA — Content scene backgrounds
BRAND.CREAM       // #F5F3F1 — card backgrounds (Content mode)
BRAND.LIGHT_GRAY  // #E0DEDC — dividers, borders
BRAND.MID_GRAY    // #999999 — labels, secondary text
BRAND.DARK_GRAY   // #555555 — body text

// Typography
BRAND.FONT_FAMILY // KMR Waldenburg with fallbacks
BRAND.LABEL_SIZE  // 16px — labels (weight 600, uppercase, tracking 0.1em)
```

### Animation System

Load `rules/animation-patterns.md` for the full reference. Summary:

- **Every animation** uses `spring({ frame, fps, config: { damping: 200 } })`
- **Cards** enter from below: `translateY(40px → 0)`, staggered 8-20 frames apart
- **Headers** enter from below: `translateY(12px → 0)`
- **List items** enter from the side: `translateX(15px → 0)`, staggered 4 frames apart
- **Badges** scale in: `scale(0 → 1)`, delay 60 frames
- **Transitions** between scenes: fade only, 45 frames (v2), linear timing
- **Never use CSS transitions or animations** — only `useCurrentFrame()` + `spring()`/`interpolate()`

### Component Patterns

Load `rules/component-patterns.md` for the full reference. Summary:

Every scene component follows this structure:

```tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, spring, interpolate, staticFile } from "remotion";
import { z } from "zod";
import { BRAND } from "../brand";
import { ChladniPattern, NoiseOverlay } from "./ChladniPattern";
import { IIWatermark } from "./IIWatermark";
import { SomeSchema } from "../schemas";

export const SomeScene: React.FC<z.infer<typeof SomeSchema>> = ({
  prop1 = "default",
  prop2 = "default",
}) => {
  const frame = useCurrentFrame();
  const { fps, durationInFrames } = useVideoConfig();

  const ENTER_DELAY = 35; // v2: content enters after crossfade overlap

  // Exit fade: content fades out 30 frames before end, leaving 10 frames of pure background
  const exitOpacity = interpolate(
    frame,
    [durationInFrames - 40, durationInFrames - 10],
    [1, 0],
    { extrapolateLeft: "clamp", extrapolateRight: "clamp" }
  );

  // Spring animations with damping: 200, delayed for v2
  const headerProgress = spring({ frame: frame - ENTER_DELAY, fps, config: { damping: 200 } });

  return (
    <AbsoluteFill style={{ backgroundColor: BRAND.OFF_WHITE }}>
      {/* Background stays full opacity — don't wrap in exitOpacity */}
      <NoiseOverlay opacity={0.07} />

      {/* Content wrapper gets exitOpacity */}
      <div style={{ opacity: exitOpacity, fontFamily: BRAND.FONT_FAMILY }}>
        {/* Scene content here */}
        <IIWatermark color="graphite" />  {/* or "white" for Hero scenes */}
      </div>
    </AbsoluteFill>
  );
};
```

For Hero scenes, wrap the background in `GradientBackground` and apply white text. The `exitOpacity` goes on the content wrapper only, never on the background:

```tsx
return (
  <AbsoluteFill>
    {/* Background: NO exitOpacity */}
    <GradientBackground src={staticFile("brand-assets/backgrounds/gradient/Background0.jpg")} />

    {/* Content: exitOpacity applied */}
    <div style={{ opacity: exitOpacity, position: "absolute", inset: 0 }}>
      {/* Hero content with white text */}
      <div style={{ opacity: exitOpacity }}>
        <IIWatermark color="white" />
      </div>
    </div>
  </AbsoluteFill>
);
```

### Schema Patterns

All schemas go in `src/schemas.ts`. Every field is `.optional()` so defaults work:

```typescript
export const SomeSchema = z.object({
  sectionLabel: z.string().optional(),
  headline: z.string().optional(),
  // ... all fields optional
});
```

### Combined Composition Pattern — MUST BE EDITABLE

Load `rules/composition-patterns.md` for the full reference. Summary:

Combined compositions MUST accept a flat Zod schema with prefixed props for every scene, so all text is editable from the Remotion Studio sidebar. Props use `{sceneName}_{propName}` naming to group by scene.

```tsx
import { TransitionSeries, linearTiming } from "@remotion/transitions";
import { fade } from "@remotion/transitions/fade";
import { z } from "zod";
import { ExampleCompositionSchema } from "../schemas";

const TRANSITION_FRAMES = 45; // v2: 1.5s crossfade

export const ExampleComposition: React.FC<z.infer<typeof ExampleCompositionSchema>> = ({
  titleCard_title = "Default Title",
  titleCard_subtitle = "Default Subtitle",
  someScene_headline = "Default Headline",
  outro_nextTitle = "Next Video Title",
}) => {
  const { fps } = useVideoConfig();
  const sec = (s: number) => s * fps;

  return (
    <AbsoluteFill>
      <TransitionSeries>
        <TransitionSeries.Sequence durationInFrames={sec(8)}>
          <WaveformSceneV2 title={titleCard_title} subtitle={titleCard_subtitle} />
        </TransitionSeries.Sequence>
        <TransitionSeries.Transition
          presentation={fade()}
          timing={linearTiming({ durationInFrames: TRANSITION_FRAMES })}
        />
        {/* ... more scenes, each receiving props from the combined schema ... */}
        <TransitionSeries.Sequence durationInFrames={sec(10)}>
          <OutroSceneV2 nextTitle={outro_nextTitle} />
        </TransitionSeries.Sequence>
      </TransitionSeries>
    </AbsoluteFill>
  );
};
```

The combined schema in `src/schemas.ts`:
```typescript
export const ExampleCompositionSchema = z.object({
  titleCard_title: z.string().optional(),
  titleCard_subtitle: z.string().optional(),
  someScene_headline: z.string().optional(),
  // ... all scene text props, all optional
  outro_nextTitle: z.string().optional(),
});
```

Only include text props in the combined schema. Structural props (feature arrays, isFocus booleans) stay hardcoded in the composition file.

**Hard cuts around placeholder scenes:** When a `FullBleedPlaceholder` scene is adjacent, do NOT add a `TransitionSeries.Transition` on either side. Hard cuts let editors cleanly swap in screen recordings in post.

### Root.tsx Registration

Each scene gets its own Composition (for individual editing) plus the combined one. The combined composition MUST have `schema` and `defaultProps`:

```tsx
const FPS = 30;
const W = 1920;
const H = 1080;
const sec = (s: number) => s * FPS;

// Combined — editable with all scene props
<Composition
  id="ExampleComposition"
  component={ExampleComposition}
  schema={ExampleCompositionSchema}
  durationInFrames={sec(totalDuration)}
  fps={FPS} width={W} height={H}
  defaultProps={{
    titleCard_title: 'Default Title',
    titleCard_subtitle: 'Default Subtitle',
    someScene_headline: 'Default Headline',
    outro_nextTitle: 'Next Video Title',
  }}
/>

// Individual scenes — also editable
<Composition
  id="v2-SomeScene"
  component={SomeScene}
  schema={SomeSchema}
  durationInFrames={sec(sceneDuration)}
  fps={FPS} width={W} height={H}
  defaultProps={{ /* from spec */ }}
/>
```

## Remotion 4.0 Specifics

This project uses Remotion 4.0.438. Key things to know:

- **Audio/Video imports:** Use `@remotion/media` for `<Audio>` and `<Video>` (not the older `remotion` package exports)
- **Font loading:** Use `@remotion/fonts` with `loadFont()` — fonts must specify `format: "opentype"` for .otf files
- **Premounting:** Always use `premountFor` prop on `<Sequence>` components
- **Static files:** Always use `staticFile()` for anything in `public/`
- **Time in seconds:** Write `s * fps`, not raw frame numbers
- **Type safety:** Use `type` not `interface` for component props; use `z.infer<typeof Schema>`
- **No CSS animations:** All motion via `useCurrentFrame()` + `spring()`/`interpolate()`

Load `rules/remotion-api.md` for the complete Remotion 4.0 API reference.

## Rules Directory

Load relevant rule files based on what you're building:

- **`rules/brand-system.md`** — Complete color, typography, and spacing reference. Load when creating any visual element.
- **`rules/animation-patterns.md`** — Spring configs, entrance animations, delay patterns, stagger timing. Load when animating anything.
- **`rules/component-patterns.md`** — Full scene component template, layout wrapping pattern, shared elements usage. Load when creating new components.
- **`rules/composition-patterns.md`** — TransitionSeries setup, duration calculation, Root.tsx registration. Load when building combined compositions.
- **`rules/remotion-api.md`** — Remotion 4.0 API essentials: hooks, components, imports, rendering. Load when you need API details.
- **`rules/spec-reading.md`** — How to parse spec files, map layouts to components, extract content fields. Load when reading a new spec.

## Remotion Best Practices (companion skill)

Always load `/remotion-best-practices` alongside this skill. It provides 30+ rule files covering the full Remotion API — animations, timing, transitions, sequencing, audio, video, 3D, text effects, charts, captions, light leaks, fonts, GIFs, Lottie, and more. This builder skill handles the ElevenLabs brand layer; best practices handles the Remotion implementation layer.

## References Directory

- **`references/existing-components.md`** — Complete inventory of all scene components, layout components, and shared elements with their props and when to use each.
- **`references/bundled-assets.md`** — Inventory of all brand assets in the project (logos, fonts, covers, icons, backgrounds).

## Common Mistakes to Avoid

1. **Creating components that already exist.** Check the inventory first. `ThreePillarsScene` with different content is still `ThreePillarsScene` — just pass different props.

2. **Making combined compositions non-editable.** Combined compositions MUST have a flat Zod schema with `{sceneName}_{propName}` prefixed props, registered with `schema` + `defaultProps` in Root.tsx. Never create a combined composition without this — it makes the full timeline uneditable in Studio.

3. **Using GRAPHITE for backgrounds.** GRAPHITE is for text and graphic elements only. Content scene backgrounds are OFF_WHITE. Hero scenes use GradientBackground.

4. **Forgetting NoiseOverlay or IIWatermark.** Every scene needs `<NoiseOverlay opacity={0.07} />`. Every scene needs `<IIWatermark color="white" />` (Hero) or `<IIWatermark color="graphite" />` (Content).

5. **Using CSS transitions.** All animation must use `useCurrentFrame()` with `spring()` or `interpolate()`. CSS transitions won't render correctly in Remotion.

6. **Hardcoding frame numbers.** Use `s * fps` for time calculations, never raw frame counts.

7. **Missing `fontFamily: BRAND.FONT_FAMILY`** on content containers.

8. **Using `interface` instead of `type`** for component props with Zod.

9. **Not making schema fields optional.** All fields should be `.optional()` with defaults in the component destructuring.

10. **V2 scenes must have ENTER_DELAY and exitOpacity.** Every v2 scene needs `const ENTER_DELAY = 35` and the `exitOpacity` interpolation applied to the content wrapper. Without these, content overlaps during transitions and exits abruptly.

11. **V2 transitions are 45 frames, not 20.** Using `TRANSITION_FRAMES = 20` with v2 scenes causes jarring cuts because the exit/entrance timing is designed for 45-frame crossfades.

12. **Don't dim the II icon on Hero scenes.** Never use `opacity: 0.4` or similar on the ElevenLabs icon in title/welcome/outro scenes. Full opacity on the icon itself — the `IIWatermark` component handles the correct opacity internally.

13. **No backdrop-filter on dense grids.** When 4+ BrandedCards are close together, use `variant="darkflat"` (no blur). Using `darkglass` or other blur variants in tight layouts causes rendering distortion artifacts.

14. **Cards should NOT stretch.** Never put `flex: 1` on BrandedCard styles — cards should hug their content. Use `alignItems: "center"` on the parent container to center cards vertically.

15. **All icons white on Hero scenes.** Use `brand-assets/icons/white/` variants. For product icons (tts.png, stt.png, convai.png), apply `filter: "brightness(0) invert(1)"`. Inline SVGs should use `stroke="#FFFFFF"`, not `stroke="#1E1916"`.

16. **Don't hardcode card text colors.** Use `getCardColors(variant)` from BrandedCard to get the correct `title`, `description`, `label`, `divider`, and `decorativeNumber` colors. Never hardcode `#151515` or `#666666` on cards.
