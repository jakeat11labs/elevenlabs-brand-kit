---
name: animations
description: Fundamental animation skills for Remotion
metadata:
  tags: animations, transitions, frames, useCurrentFrame
---

# Fundamental Animation Rules

## Core Requirement

All animations MUST be driven by the `useCurrentFrame()` hook.

## Time Calculation

Animations should be written in seconds, then multiplied by the `fps` value obtained from `useVideoConfig()`.

```tsx
import { useCurrentFrame, useVideoConfig, interpolate } from "remotion";

export const FadeIn = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const opacity = interpolate(frame, [0, 2 * fps], [0, 1], {
    extrapolateRight: "clamp",
  });

  return <div style={{ opacity }}>Hello World!</div>;
};
```

## Forbidden Practices

- CSS transitions and animations — will fail to render correctly
- Tailwind animation class names — will fail to render correctly
- `useFrame()` from React Three Fiber — causes flickering
