---
name: text-animations
description: Typography and text animation patterns for Remotion
metadata:
  tags: typography, text, typewriter, highlighter
---

# Text Animations

## Typewriter Effect

Use string slicing based on `useCurrentFrame()`. Never use per-character opacity.

```tsx
import { useCurrentFrame, useVideoConfig } from "remotion";

export const Typewriter = ({ text }: { text: string }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Reveal ~20 characters per second
  const charsToShow = Math.floor((frame / fps) * 20);
  const visible = text.slice(0, charsToShow);

  return (
    <div>
      {visible}
      <span style={{ opacity: Math.floor(frame / 15) % 2 === 0 ? 1 : 0 }}>|</span>
    </div>
  );
};
```

Key rule: **always use string slicing, never per-character opacity**.

## Word Highlighting

Animate a highlight element behind/over a word using `interpolate()` or `spring()`:

```tsx
import { interpolate, useCurrentFrame, useVideoConfig } from "remotion";

export const WordHighlight = ({ word }: { word: string }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const progress = interpolate(frame, [0, 0.5 * fps], [0, 1], {
    extrapolateRight: "clamp",
  });

  return (
    <div style={{ position: "relative", display: "inline-block" }}>
      <div
        style={{
          position: "absolute",
          background: "yellow",
          width: `${progress * 100}%`,
          height: "100%",
          opacity: 0.6,
        }}
      />
      <span style={{ position: "relative" }}>{word}</span>
    </div>
  );
};
```
