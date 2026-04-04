---
name: timing
description: Interpolation curves in Remotion - linear, easing, spring animations
metadata:
  tags: timing, interpolate, spring, easing, animation
---

# Timing and Animation Curves

## Linear Interpolation

`interpolate()` maps values across time ranges, with optional clamping.

```tsx
import { interpolate, useCurrentFrame } from "remotion";

const frame = useCurrentFrame();
const opacity = interpolate(frame, [0, 30], [0, 1], {
  extrapolateRight: "clamp",
});
```

## Spring Animations

Physics-based motion from 0 to 1 over time. Default config: `mass: 1, damping: 10, stiffness: 100` (bouncy).

For smooth animations without bounce, use `damping: 200`:

```tsx
import { spring, useCurrentFrame, useVideoConfig } from "remotion";

const frame = useCurrentFrame();
const { fps } = useVideoConfig();

const scale = spring({ frame, fps, config: { damping: 200 } });
```

## Spring Configuration Presets

- **smooth**: `damping: 200` — subtle reveals
- **snappy**: `damping: 20, stiffness: 200` — UI elements
- **bouncy**: `damping: 8` — playful effects
- **heavy**: `damping: 15, stiffness: 80, mass: 2` — slow, weighted motion

## Delay & Duration

```tsx
const scale = spring({
  frame,
  fps,
  delay: 15, // frames
  durationInFrames: 45,
  config: { damping: 200 },
});
```

## Easing Functions

Applied to `interpolate()` using combinations:
- convexities: `in`, `out`, `inOut`
- curves: `quad`, `sin`, `exp`, `circle`
- cubic Bézier: `Easing.bezier()`

```tsx
import { interpolate, Easing, useCurrentFrame } from "remotion";

const frame = useCurrentFrame();
const x = interpolate(frame, [0, 60], [0, 500], {
  easing: Easing.inOut(Easing.quad),
  extrapolateRight: "clamp",
});
```

## Combining Springs with Interpolation

```tsx
const progress = spring({ frame, fps, config: { damping: 200 } });
const translateY = interpolate(progress, [0, 1], [50, 0]);
const opacity = interpolate(progress, [0, 1], [0, 1]);
```
