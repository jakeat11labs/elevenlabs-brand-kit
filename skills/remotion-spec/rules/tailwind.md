---
name: tailwind
description: Using TailwindCSS in Remotion
metadata:
  tags: tailwind, css, styling
---

# TailwindCSS in Remotion

You can and should use TailwindCSS in Remotion if it is installed in the project.

## Important Restriction

Do NOT use Tailwind's `transition` or `animate` utility classes. These use CSS animations which will fail to render correctly.

Instead, drive all animations through `useCurrentFrame()`:

```tsx
// ❌ Wrong
<div className="animate-fade-in transition-opacity duration-300">

// ✅ Correct
const opacity = interpolate(frame, [0, 15], [0, 1], { extrapolateRight: "clamp" });
<div className="text-white font-bold" style={{ opacity }}>
```

## Setup

TailwindCSS must be properly installed and configured in the Remotion project. See: https://www.remotion.dev/docs/tailwind

Use Tailwind for: layout, spacing, colors, typography, borders, backgrounds — anything static.
Use `useCurrentFrame()` for: opacity, transforms, size changes, colors that animate over time.
