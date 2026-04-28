---
name: transitions
description: Scene transition patterns for Remotion using TransitionSeries
metadata:
  tags: transitions, TransitionSeries, fade, slide, wipe, flip
---

# Transitions

Install: `npx remotion add @remotion/transitions`

## Basic Usage

```tsx
import { TransitionSeries, linearTiming } from "@remotion/transitions";
import { fade } from "@remotion/transitions/fade";

<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneA />
  </TransitionSeries.Sequence>
  <TransitionSeries.Transition
    presentation={fade()}
    timing={linearTiming({ durationInFrames: 15 })}
  />
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneB />
  </TransitionSeries.Sequence>
</TransitionSeries>
```

## Available Transitions

```tsx
import { fade } from "@remotion/transitions/fade";
import { slide } from "@remotion/transitions/slide";      // direction: "from-left" | "from-right" | "from-top" | "from-bottom"
import { wipe } from "@remotion/transitions/wipe";
import { flip } from "@remotion/transitions/flip";
import { clockWipe } from "@remotion/transitions/clock-wipe";
```

## Timing Options

```tsx
import { linearTiming, springTiming } from "@remotion/transitions";

linearTiming({ durationInFrames: 15 })
springTiming({ config: { damping: 200 } })
```

## Duration Calculation

Two 60-frame sequences with a 15-frame transition = **105 frames total** (not 135):

```tsx
import { getDurationInFrames } from "@remotion/transitions";

const totalDuration = getDurationInFrames({
  durationOfEachScene: [60, 60],
  transitions: [{ durationInFrames: 15 }],
});
```

## Overlays

Overlays render effects on top of cut points without affecting timeline length:

```tsx
<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneA />
  </TransitionSeries.Sequence>
  <TransitionSeries.Overlay durationInFrames={20}>
    <LightLeak />
  </TransitionSeries.Overlay>
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneB />
  </TransitionSeries.Sequence>
</TransitionSeries>
```

Note: Overlays cannot be adjacent to transitions or other overlays.
