# Remotion 4.0 API Reference

This project uses Remotion 4.0.438 with React 19 and Zod 4.

## Core Imports

```tsx
// Most common imports — from "remotion"
import {
  AbsoluteFill,
  useCurrentFrame,
  useVideoConfig,
  spring,
  interpolate,
  staticFile,
  Img,
  Sequence,
  Series,
  Composition,
  Still,
  Folder,
} from "remotion";

// Transitions — from "@remotion/transitions"
import { TransitionSeries, linearTiming, springTiming } from "@remotion/transitions";
import { fade } from "@remotion/transitions/fade";
import { slide } from "@remotion/transitions/slide";

// Audio/Video — from "@remotion/media" (Remotion 4.0+)
import { Audio } from "@remotion/media";
import { Video } from "@remotion/media";

// Fonts — from "@remotion/fonts"
import { loadFont } from "@remotion/fonts";

// Schemas — from "zod"
import { z } from "zod";
```

## useCurrentFrame()

Returns the current frame number (starts at 0). Inside a `<Sequence>`, returns the LOCAL frame (relative to sequence start).

```tsx
const frame = useCurrentFrame();
```

## useVideoConfig()

Returns composition configuration:

```tsx
const { fps, width, height, durationInFrames } = useVideoConfig();
```

## interpolate()

Maps a value from one range to another:

```tsx
const opacity = interpolate(frame, [0, 30], [0, 1], {
  extrapolateRight: "clamp",  // Prevents values beyond [0, 1]
});

// Multi-point interpolation
const x = interpolate(frame, [0, 30, 60], [0, 100, 100]);
```

Always use `extrapolateRight: "clamp"` when you don't want values to exceed the output range.

## spring()

Physics-based animation from 0 to 1:

```tsx
const progress = spring({
  frame,
  fps,
  delay: 15,        // frames (optional)
  config: {
    damping: 200,    // ALWAYS 200 in this project
  },
});
```

Our project standard: `config: { damping: 200 }` — snappy, no bounce.

## staticFile()

References files in the `public/` directory:

```tsx
<Img src={staticFile("logos/Logo-black.png")} />
<Audio src={staticFile("audio/narration.mp3")} />
```

## Img

Use `<Img>` (not HTML `<img>`) — ensures image is loaded before rendering:

```tsx
<Img src={staticFile("photo.jpg")} style={{ width: "100%", objectFit: "cover" }} />
```

## AbsoluteFill

Fills the entire composition area. Use as root of every scene:

```tsx
<AbsoluteFill style={{ backgroundColor: BRAND.OFF_WHITE }}>
  {/* Scene content */}
</AbsoluteFill>
```

## Sequence

Delays when an element appears. Always use `premountFor`:

```tsx
<Sequence from={1 * fps} durationInFrames={2 * fps} premountFor={1 * fps}>
  <Component />
</Sequence>
```

Inside a Sequence, `useCurrentFrame()` returns local frame (starting from 0).

## TransitionSeries

Manages scene transitions:

```tsx
<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={sec(8)}>
    <SceneA />
  </TransitionSeries.Sequence>
  <TransitionSeries.Transition
    presentation={fade()}
    timing={linearTiming({ durationInFrames: 20 })}
  />
  <TransitionSeries.Sequence durationInFrames={sec(6)}>
    <SceneB />
  </TransitionSeries.Sequence>
</TransitionSeries>
```

Duration calculation: transitions cause overlap. Two 60-frame sequences with a 15-frame transition = 105 frames total (not 120).

**Important constraint:** The duration of a sequence must not be shorter than the duration of the preceding transition.

## Composition

Registers a video in Root.tsx:

```tsx
<Composition
  id="UniqueId"
  component={MyComponent}
  schema={MySchema}           // optional — for Studio editing
  durationInFrames={sec(10)}
  fps={30}
  width={1920}
  height={1080}
  defaultProps={{ title: "Hello" }}
/>
```

## Audio (Remotion 4.0)

```tsx
import { Audio } from "@remotion/media";

<Audio src={staticFile("audio/narration.mp3")} />
<Audio src={staticFile("audio/bg-music.mp3")} volume={0.3} />
```

## Zod Schemas

Define editable props for Remotion Studio:

```tsx
import { z } from "zod";

export const MySchema = z.object({
  title: z.string().optional(),
  items: z.array(z.object({
    label: z.string(),
    icon: z.string().optional(),
  })).optional(),
});

// In component:
const MyComponent: React.FC<z.infer<typeof MySchema>> = ({ title = "Default" }) => { ... };
```

Use `type` (not `interface`) for props — required for Zod type inference.

## Font Loading

```tsx
import { loadFont } from "@remotion/fonts";
import { staticFile } from "remotion";

const font = loadFont({
  family: "KMR Waldenburg",
  url: staticFile("fonts/KMR-Waldenburg-Buch.otf"),
  format: "opentype",  // REQUIRED for .otf files
  weight: "300",
});
```

## Rendering

```bash
# Preview in Studio
npx remotion studio

# Render a specific composition
npx remotion render M01L01v3 out/m01-l01.mp4

# With angle backend (for GPU)
npx remotion render M01L01v3 out/m01-l01.mp4 --gl=angle
```

## React 19 Notes

This project uses React 19. Key differences:
- No need for `React.FC` import (it's available globally)
- Simplified ref forwarding
- Use `ref` as a prop directly instead of `forwardRef`
