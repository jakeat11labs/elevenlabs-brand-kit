---
name: charts
description: Chart and data visualization patterns for Remotion (bar, pie, line, stock charts)
metadata:
  tags: charts, data, visualization, bar, pie, line
---

# Charts in Remotion

Disable all third-party animation libraries. Drive everything through `useCurrentFrame()`.

## Bar Charts

Use `spring()` with staggered delays:

```tsx
import { spring, useCurrentFrame, useVideoConfig } from "remotion";

const frame = useCurrentFrame();
const { fps } = useVideoConfig();

const data = [30, 60, 45, 80, 55];

{data.map((value, i) => {
  const height = spring({
    frame: frame - i * 5, // stagger
    fps,
    config: { damping: 200 },
  }) * value;

  return <div key={i} style={{ height, width: 40, background: "white" }} />;
})}
```

## Pie Charts

Animate via `stroke-dashoffset`:

```tsx
const circumference = 2 * Math.PI * radius;
const frame = useCurrentFrame();
const { fps } = useVideoConfig();

const progress = spring({ frame, fps, config: { damping: 200 } });
const offset = circumference * (1 - progress * segmentPercent);

<circle
  r={radius}
  strokeDasharray={circumference}
  strokeDashoffset={offset}
  transform={`rotate(-90, cx, cy)`}
/>
```

## Line Charts / Path Animation

Requires `@remotion/paths`: `npx remotion add @remotion/paths`

```tsx
import { evolvePath, getLength, getPointAtLength, getTangentAtLength } from "@remotion/paths";
import { interpolate, useCurrentFrame, useVideoConfig } from "remotion";

const frame = useCurrentFrame();
const { fps } = useVideoConfig();

// Convert data to SVG path
const points = data.map((y, x) => ({ x: x * 50, y: 200 - y }));
const d = points
  .map((p, i) => `${i === 0 ? "M" : "L"} ${p.x} ${p.y}`)
  .join(" ");

// Animate path drawing
const progress = interpolate(frame, [0, 2 * fps], [0, 1], {
  extrapolateRight: "clamp",
});
const { strokeDasharray, strokeDashoffset } = evolvePath(progress, d);

<path d={d} strokeDasharray={strokeDasharray} strokeDashoffset={strokeDashoffset} />

// Animate a marker along the path
const pathLength = getLength(d);
const point = getPointAtLength(d, pathLength * progress);
const tangent = getTangentAtLength(d, pathLength * progress);
const angle = Math.atan2(tangent.y, tangent.x) * (180 / Math.PI);
```
