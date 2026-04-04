---
name: light-leaks
description: Light leak overlay effects using @remotion/light-leaks
metadata:
  tags: light-leaks, transitions, overlay, effects
---

# Light Leaks

Requires Remotion 4.0.415+. Install: `npx remotion add @remotion/light-leaks`

## In Transitions

```tsx
import { TransitionSeries, linearTiming } from "@remotion/transitions";
import { LightLeak } from "@remotion/light-leaks";

<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneA />
  </TransitionSeries.Sequence>
  <TransitionSeries.Overlay durationInFrames={30}>
    <LightLeak seed={0} hueShift={0} />
  </TransitionSeries.Overlay>
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneB />
  </TransitionSeries.Sequence>
</TransitionSeries>
```

## Standalone Overlay

```tsx
import { AbsoluteFill } from "remotion";
import { LightLeak } from "@remotion/light-leaks";

<AbsoluteFill>
  <MyScene />
  <LightLeak seed={2} hueShift={120} />
</AbsoluteFill>
```

## Props

- `seed` — Pattern shape (default: 0). Different values = different patterns.
- `hueShift` — Color in degrees 0–360 (default: 0 = yellow-orange; 120 = green; 240 = blue)
- `durationInFrames` — Defaults to parent duration. Controls reveal (first half) and retract (second half).
