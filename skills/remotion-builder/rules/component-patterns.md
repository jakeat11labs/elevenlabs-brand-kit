# Component Patterns

## Scene Component Template

Every scene component follows this exact structure. Copy this when creating new ones:

```tsx
import {
  AbsoluteFill,
  useCurrentFrame,
  useVideoConfig,
  spring,
  interpolate,
  staticFile,
  Img,
} from "remotion";
import { z } from "zod";
import { BRAND } from "../brand";
import { ChladniPattern, NoiseOverlay } from "./ChladniPattern";
import { IIWatermark } from "./IIWatermark";
import { NewSceneSchema } from "../schemas";

export const NewScene: React.FC<z.infer<typeof NewSceneSchema>> = ({
  sectionLabel = "DEFAULT LABEL",
  headline = "Default Headline",
  // ... all props with defaults
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Animation springs — damping: 200 always
  const headerProgress = spring({ frame, fps, config: { damping: 200 } });
  const lineProgress = spring({ frame, fps, delay: 6, config: { damping: 200 } });

  return (
    <AbsoluteFill style={{ backgroundColor: BRAND.OFF_WHITE }}>
      {/* Background pattern — subtle */}
      <ChladniPattern
        opacity={0.03}
        variant="geometric"
        position="right"
        color={BRAND.LIGHT_GRAY}
      />
      <NoiseOverlay opacity={0.04} />

      {/* Content container */}
      <div
        style={{
          padding: "60px 100px",
          display: "flex",
          flexDirection: "column",
          height: "100%",
          fontFamily: BRAND.FONT_FAMILY,
        }}
      >
        {/* Section header — label + headline + divider */}
        <div style={{ marginBottom: 32 }}>
          <div
            style={{
              opacity: interpolate(headerProgress, [0, 1], [0, 1]),
              fontSize: BRAND.LABEL_SIZE,
              fontWeight: 600,
              letterSpacing: "0.1em",
              textTransform: "uppercase" as const,
              color: BRAND.MID_GRAY,
              marginBottom: 10,
            }}
          >
            {sectionLabel}
          </div>
          <div
            style={{
              opacity: interpolate(headerProgress, [0, 1], [0, 1]),
              transform: `translateY(${interpolate(headerProgress, [0, 1], [12, 0])}px)`,
              fontSize: 44,
              fontWeight: 300,
              letterSpacing: "-0.02em",
              color: BRAND.GRAPHITE,
              marginBottom: 14,
            }}
          >
            {headline}
          </div>
          <div
            style={{
              width: `${100 * lineProgress}%`,
              height: 1,
              backgroundColor: BRAND.LIGHT_GRAY,
            }}
          />
        </div>

        {/* Main content area — fills remaining space */}
        <div style={{ display: "flex", gap: 28, flex: 1 }}>
          {/* Cards, lists, or other content */}
        </div>
      </div>

      <IIWatermark />
    </AbsoluteFill>
  );
};
```

## Layout Component vs Scene Component

**Scene components** (`src/components/`):
- Full 1920×1080 compositions
- Include their own `<AbsoluteFill>`, background, NoiseOverlay, IIWatermark
- Registered directly as `<Composition>` in Root.tsx
- Examples: `WaveformScene`, `ThreePillarsScene`, `FoundationDiagram`

**Layout components** (`src/components/layouts/`):
- Content-area patterns (cards, grids, lists)
- Already wrapped with background, noise, and watermark
- Can be used directly as scene components
- Examples: `TwoColumnCards`, `FourGridCards`, `StatNumber`

Both follow the same animation and brand patterns. The distinction is organizational.

## Wrapping a Layout in a Scene Shell

If you need a layout that already exists but want to add a custom header or behavior:

```tsx
export const CustomScene: React.FC<Props> = (props) => {
  return (
    <AbsoluteFill style={{ backgroundColor: BRAND.OFF_WHITE }}>
      <NoiseOverlay opacity={0.04} />
      {/* Custom header or wrapper */}
      <ExistingLayout {...props} />
      <IIWatermark />
    </AbsoluteFill>
  );
};
```

But note: most layout components already include their own AbsoluteFill, NoiseOverlay, and IIWatermark. Check the existing code before wrapping.

## Handling \n Line Breaks

Spec files use `\n` in titles and subtitles. Handle them like this:

```tsx
{title.split("\n").map((line, i) => (
  <span key={i}>
    {line}
    {i < title.split("\n").length - 1 && <br />}
  </span>
))}
```

## Passing Icon References

Icons referenced in specs map to `staticFile()` paths:

```tsx
// Spec says: "icons/tts.png"
<Img src={staticFile("icons/tts.png")} style={{ width: 48, height: 48 }} />

// Some cards have no icon — check for it
{icon && <Img src={staticFile(icon)} style={{ width: 48, height: 48 }} />}
```

## Inline SVG Icons

For simple icons (question marks, chat bubbles, checkmarks), use inline SVG rather than image files:

```tsx
// Question mark icon
<svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke={BRAND.GRAPHITE} strokeWidth="1.5">
  <circle cx="12" cy="12" r="10" />
  <path d="M9 9c0-1.66 1.34-3 3-3s3 1.34 3 3c0 2-3 1.75-3 5" />
  <circle cx="12" cy="17" r="0.5" fill={BRAND.GRAPHITE} />
</svg>
```

## Content That Lives in the Component vs Props

**In props (schema):** Text that changes between lessons — headlines, labels, descriptions, feature lists
**Hardcoded in component:** Visual structure, icon choices, card layouts, animation timing, specific data arrays (like the PILLARS array in ThreePillarsScene)

This is a judgment call. If the spec uses the same component with different content, props make sense. If the data is specific to one scene, hardcode it.

## Creating Schemas

All schemas go in `src/schemas.ts`. Pattern:

```typescript
export const NewSceneSchema = z.object({
  sectionLabel: z.string().optional(),
  headline: z.string().optional(),
  items: z.array(z.object({
    title: z.string(),
    description: z.string(),
    icon: z.string().optional(),
  })).optional(),
  isFocus: z.boolean().optional(),
});
```

- All fields `.optional()` — defaults are in the component
- Arrays of objects for card/item content
- Booleans for feature flags (isFocus, etc.)
- Use `z.string()` for icon paths

## V2 Scene Component Template

V2 Hero scene components follow this structure:

```tsx
import {
  AbsoluteFill, Img, useCurrentFrame, useVideoConfig,
  spring, interpolate, staticFile,
} from "remotion";
import { z } from "zod";
import { BRAND } from "../../brand";
import { GradientBackground } from "./GradientBackground";
import { BrandedCard } from "./BrandedCard";
import { IIWatermark } from "../IIWatermark";
import { NewSceneV2Schema } from "../../schemas";

export const NewSceneV2: React.FC<z.infer<typeof NewSceneV2Schema>> = ({
  sectionLabel = "DEFAULT LABEL",
  headline = "Default Headline",
  backgroundSrc = "brand-assets/backgrounds/gradient/Background6.jpg",
}) => {
  const frame = useCurrentFrame();
  const { fps, durationInFrames } = useVideoConfig();

  // Exit fade: content fades out during last 30 frames
  const exitOpacity = interpolate(
    frame,
    [durationInFrames - 40, durationInFrames - 10],
    [1, 0],
    { extrapolateLeft: "clamp", extrapolateRight: "clamp" }
  );

  const ENTER_DELAY = 35;

  const headerProgress = spring({ frame, fps, delay: ENTER_DELAY, config: { damping: 200 } });
  const lineProgress = spring({ frame, fps, delay: ENTER_DELAY + 6, config: { damping: 200 } });

  return (
    <AbsoluteFill>
      <GradientBackground src={backgroundSrc} />

      {/* Content — fades out before scene transition */}
      <div
        style={{
          padding: "60px 100px",
          display: "flex",
          flexDirection: "column",
          height: "100%",
          fontFamily: BRAND.FONT_FAMILY,
          position: "relative",
          zIndex: 1,
          opacity: exitOpacity,
        }}
      >
        {/* Header — white text on gradient */}
        <div style={{ marginBottom: 32 }}>
          <div style={{
            opacity: interpolate(headerProgress, [0, 1], [0, 1]),
            fontSize: BRAND.LABEL_SIZE, fontWeight: 600,
            letterSpacing: "0.1em", textTransform: "uppercase",
            color: "rgba(255,255,255,0.6)", marginBottom: 10,
          }}>
            {sectionLabel}
          </div>
          <div style={{
            opacity: interpolate(headerProgress, [0, 1], [0, 1]),
            transform: `translateY(${interpolate(headerProgress, [0, 1], [12, 0])}px)`,
            fontSize: 52, fontWeight: 300, letterSpacing: "-0.02em",
            color: BRAND.WHITE, marginBottom: 14,
          }}>
            {headline}
          </div>
          <div style={{
            width: `${100 * spring({ frame, fps, delay: ENTER_DELAY + 6, config: { damping: 200 } })}%`,
            height: 1, backgroundColor: "rgba(255,255,255,0.3)",
          }} />
        </div>

        {/* Main content — frosted cards */}
        <div style={{ display: "flex", gap: 28, flex: 1 }}>
          {items.map((item, i) => (
            <BrandedCard key={i} style={{
              opacity: interpolate(spring({ frame, fps, delay: ENTER_DELAY + 15 + i * 25, config: { damping: 200 } }), [0, 1], [0, 1]),
              transform: `translateY(${interpolate(spring({ frame, fps, delay: ENTER_DELAY + 15 + i * 25, config: { damping: 200 } }), [0, 1], [40, 0])}px)`,
              flex: 1, padding: "48px 40px",
            }}>
              {/* Card content — dark text on frosted cream */}
            </BrandedCard>
          ))}
        </div>
      </div>

      <div style={{ opacity: exitOpacity }}>
        <IIWatermark color="white" />
      </div>
    </AbsoluteFill>
  );
};
```

### V2 Hybrid Split Template (PillarDetail/Timeline style)

White left side with GRAPHITE text, gradient right side with frosted cards:

```tsx
<AbsoluteFill style={{ backgroundColor: BRAND.OFF_WHITE }}>
  {/* Right side — gradient + frosted cards */}
  <div style={{ position: "absolute", right: 0, top: 0, bottom: 0, width: "55%" }}>
    <Img src={staticFile(backgroundSrc)} style={{ width: "100%", height: "100%", objectFit: "cover" }} />
    {/* Noise overlay + content on gradient */}
  </div>

  {/* Left side — white with GRAPHITE text */}
  <div style={{ position: "absolute", left: 0, top: 0, bottom: 0, width: "45%", justifyContent: "center" }}>
    {/* Banner, tagline, description */}
  </div>

  <IIWatermark color="graphite" />
</AbsoluteFill>
```
