# Composition Patterns

## Combined Composition File

Each lesson gets a combined composition that stitches all scenes together with TransitionSeries. This is the file you'd render for the full lesson video.

### File Naming
`src/M{XX}L{YY}v{Z}.tsx` — e.g., `src/M01L01v3.tsx`, `src/M03L02v1.tsx`

### Template

Combined compositions MUST be editable — they accept a flat Zod schema with prefixed props for every scene, so all text is editable from the Remotion Studio sidebar when previewing the full timeline.

```tsx
import { AbsoluteFill, useVideoConfig } from "remotion";
import { TransitionSeries, linearTiming } from "@remotion/transitions";
import { fade } from "@remotion/transitions/fade";
import { z } from "zod";

import "../brand";  // Ensure fonts are loaded

// Import all scene components
import { WaveformScene } from "./components/WaveformScene";
import { OutroScene } from "./components/OutroScene";
// ... other scenes
import { M01L01v3Schema } from "../schemas";

// Lesson metadata comment
// M{XX} L{YY} v{Z} — [Lesson Title] (~[duration] VO)
// [scene count] scenes matching every beat of the voiceover
// All light (OFF_WHITE). Fade transitions only.
// Every scene's text is editable via the Studio sidebar.

const TRANSITION_FRAMES = 20;

export const M01L01v3: React.FC<z.infer<typeof M01L01v3Schema>> = ({
  // Scene 1 — Title Card
  titleCard_title = "Welcome to\nElevenUniversity",
  titleCard_subtitle = "Module 1 — The Platform\nAgent Builder Certification",

  // Scene 2 — Some Scene
  someScene_sectionLabel = "Default Label",
  someScene_headline = "Default Headline",

  // ... all scenes get props with defaults

  // Scene N — Outro
  outro_lessonNumber = "Lesson 1.2",
  outro_lessonTitle = "Next Lesson Title",
}) => {
  const { fps } = useVideoConfig();
  const sec = (s: number) => s * fps;

  return (
    <AbsoluteFill>
      <TransitionSeries>
        {/* 1. Title Card — "[VO beat]" */}
        <TransitionSeries.Sequence durationInFrames={sec(8)}>
          <WaveformScene
            title={titleCard_title}
            subtitle={titleCard_subtitle}
          />
        </TransitionSeries.Sequence>

        <TransitionSeries.Transition
          presentation={fade()}
          timing={linearTiming({ durationInFrames: TRANSITION_FRAMES })}
        />

        {/* 2. [Scene Name] — "[VO beat]" */}
        <TransitionSeries.Sequence durationInFrames={sec(6)}>
          <SomeScene
            sectionLabel={someScene_sectionLabel}
            headline={someScene_headline}
          />
        </TransitionSeries.Sequence>

        <TransitionSeries.Transition
          presentation={fade()}
          timing={linearTiming({ durationInFrames: TRANSITION_FRAMES })}
        />

        {/* ... more scenes, each with a transition between them ... */}

        {/* N. Outro — "Up next..." */}
        <TransitionSeries.Sequence durationInFrames={sec(10)}>
          <OutroScene
            lessonNumber={outro_lessonNumber}
            lessonTitle={outro_lessonTitle}
          />
        </TransitionSeries.Sequence>
      </TransitionSeries>
    </AbsoluteFill>
  );
};
```

### Key Rules

1. **Every scene** gets a `TransitionSeries.Sequence` with `durationInFrames={sec(duration)}`
2. **Every pair of scenes** has a `TransitionSeries.Transition` between them
3. **Transitions are always** `fade()` with `linearTiming({ durationInFrames: 20 })`
4. **The last sequence** (Outro) has NO transition after it
5. **Comments** on each sequence include the scene number, name, and VO beat
6. **Import `"../brand"`** at the top to ensure font loading

### Duration Calculation

Transitions overlap scenes, reducing total duration:

```
sceneDurations = [8, 6, 8, 6, 5, 6, 6, 6, 6, 6, 8, 18, 10]  // 13 scenes = 99s
numTransitions = 12  // one fewer than scenes
overlapSeconds = 12 * (20 / 30) = 8  // 20 frames at 30fps
actualDuration = 99 - 8 = ~91 seconds
```

For the Root.tsx registration, use the **sum of scene durations** (not actual) to ensure all content fits:

```tsx
<Composition
  id="M01L01v3"
  component={M01L01v3}
  durationInFrames={sec(110)}  // slightly padded
  fps={FPS} width={W} height={H}
/>
```

## Root.tsx Registration

### Combined Composition (MUST have schema + defaultProps)

The combined composition MUST be registered with its schema and defaultProps so every scene's text is editable in Remotion Studio:

```tsx
<Composition
  id="M01L01v3"
  component={M01L01v3}
  schema={M01L01v3Schema}
  durationInFrames={sec(totalDuration)}
  fps={FPS} width={W} height={H}
  defaultProps={{
    titleCard_title: 'Welcome to\nElevenUniversity',
    titleCard_subtitle: 'Module 1 — The Platform\nAgent Builder Certification',
    someScene_sectionLabel: 'Default Label',
    someScene_headline: 'Default Headline',
    // ... all scene props with defaults
    outro_lessonNumber: 'Lesson 1.2',
    outro_lessonTitle: 'Next Lesson Title',
  }}
/>
```

### Individual Scene Compositions

Each scene also gets registered individually for preview/editing in Remotion Studio:

```tsx
<Composition
  id="v3-TitleCard"
  component={WaveformScene}
  schema={WaveformSceneSchema}
  durationInFrames={sec(8)}
  fps={FPS} width={W} height={H}
  defaultProps={{
    title: 'Welcome to\nElevenUniversity',
    subtitle: 'Module 1 — The Platform\nAgent Builder Certification',
  }}
/>
```

### ID Naming Convention

- Combined: `M{XX}L{YY}v{Z}` — e.g., `M01L01v3`
- Individual: `v{Z}-{SceneName}` — e.g., `v3-TitleCard`, `v3-ThreePillars`
- If a lesson reuses a component (e.g., PillarDetailScene 3 times), add a suffix: `v3-PillarDetail-Creative`, `v3-PillarDetail-Agents`, `v3-PillarDetail-API`

### Standard Constants

```tsx
const FPS = 30;
const W = 1920;
const H = 1080;
const sec = (s: number) => s * FPS;
```

### Passing Props — Combined Composition is Editable

The combined composition uses a **flat schema with prefixed prop names** to make every scene editable in Studio. Props flow from the combined schema through to each scene component:

```tsx
// Combined composition accepts flat prefixed props and passes them through
export const M02L01v1: React.FC<z.infer<typeof M02L01v1Schema>> = ({
  titleCard_title = "Default Title",
  someScene_headline = "Default Headline",
  outro_lessonNumber = "Lesson 2.2",
}) => {
  return (
    <TransitionSeries>
      <TransitionSeries.Sequence durationInFrames={sec(8)}>
        <WaveformScene title={titleCard_title} />
      </TransitionSeries.Sequence>
      {/* ... */}
      <TransitionSeries.Sequence durationInFrames={sec(6)}>
        <SomeScene headline={someScene_headline} />
      </TransitionSeries.Sequence>
      {/* ... */}
    </TransitionSeries>
  );
};
```

### Prop Naming Convention for Combined Schemas

Use `{sceneName}_{propName}` format — this groups fields by scene in the Studio sidebar:

```
titleCard_title, titleCard_subtitle
textReveal_sectionLabel, textReveal_headline, textReveal_line1
origin_label, origin_title, origin_subtitle
outro_lessonNumber, outro_lessonTitle
```

### Individual Scene Registrations

Each scene also gets registered individually for isolated preview/editing:

```tsx
<Composition
  id="v3-PillarDetail-Creative"
  component={PillarDetailScene}
  schema={PillarDetailSchema}
  durationInFrames={sec(6)}
  fps={FPS} width={W} height={H}
  defaultProps={{
    name: 'ElevenCreative',
    tagline: 'The Studio',
    description: 'Text-to-speech, dubbing...',
    features: [{ icon: 'icons/tts.png', label: 'Text to Speech' }],
  }}
/>
```

### Combined Schema Pattern

The combined schema goes in `src/schemas.ts` alongside individual scene schemas:

```typescript
export const M02L01v1Schema = z.object({
  // Scene 1 — Title Card
  titleCard_title: z.string().optional(),
  titleCard_subtitle: z.string().optional(),

  // Scene 2 — Some Scene
  someScene_sectionLabel: z.string().optional(),
  someScene_headline: z.string().optional(),

  // ... every scene's text props, all optional

  // Scene N — Outro
  outro_lessonNumber: z.string().optional(),
  outro_lessonTitle: z.string().optional(),
});
```

All fields are `.optional()` with defaults in the component destructuring. Only include **text props** that someone would want to edit — structural props like `features` arrays or `isFocus` booleans stay hardcoded in the composition file.

### V2 Composition Differences

V2 compositions use:
- `TRANSITION_FRAMES = 45` (not 20)
- V2 scene components from `src/components/v2/`
- Minimum 8s per scene duration
- Import `GradientBackground`, `BrandedCard` etc. from `./components/v2/`

```tsx
// V2 composition template differences
const TRANSITION_FRAMES = 45;  // 1.5s crossfade (v1 was 20 frames)

// Import v2 components
import { WaveformSceneV2 } from "./components/v2/WaveformSceneV2";
import { OutroSceneV2 } from "./components/v2/OutroSceneV2";
// etc.
```

## When Updating an Existing Composition

1. Read the updated spec
2. Compare with the current composition code
3. Add/remove/reorder TransitionSeries.Sequence entries
4. Update props passed to scene components
5. Add new imports if new components were created
6. Update the Root.tsx registrations (add new, remove old)
7. Update the combined composition's durationInFrames
8. Run `npx remotion studio` to verify
