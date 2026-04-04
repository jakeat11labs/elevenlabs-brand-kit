---
name: sequencing
description: Sequencing patterns for Remotion - delay, trim, limit duration of items
metadata:
  tags: sequence, series, timing, delay, trim
---

# Sequencing Patterns

Use `<Sequence>` to delay when an element appears in the timeline.

```tsx
import { Sequence } from "remotion";

const { fps } = useVideoConfig();

<Sequence from={1 * fps} durationInFrames={2 * fps} premountFor={1 * fps}>
  <Title />
</Sequence>
<Sequence from={2 * fps} durationInFrames={2 * fps} premountFor={1 * fps}>
  <Subtitle />
</Sequence>
```

This wraps the component in an absolute fill by default. To disable wrapping:

```tsx
<Sequence layout="none">
  <Title />
</Sequence>
```

## Premounting

Always premount any `<Sequence>` — loads the component before it plays:

```tsx
<Sequence premountFor={1 * fps}>
  <Title />
</Sequence>
```

## Series

Use `<Series>` when elements should play one after another without overlap:

```tsx
import { Series } from "remotion";

<Series>
  <Series.Sequence durationInFrames={45}>
    <Intro />
  </Series.Sequence>
  <Series.Sequence durationInFrames={60}>
    <MainContent />
  </Series.Sequence>
  <Series.Sequence durationInFrames={30}>
    <Outro />
  </Series.Sequence>
</Series>;
```

### Series with overlaps

```tsx
<Series>
  <Series.Sequence durationInFrames={60}>
    <SceneA />
  </Series.Sequence>
  <Series.Sequence offset={-15} durationInFrames={60}>
    {/* Starts 15 frames before SceneA ends */}
    <SceneB />
  </Series.Sequence>
</Series>
```

## Frame References Inside Sequences

Inside a Sequence, `useCurrentFrame()` returns the local frame (starting from 0):

```tsx
<Sequence from={60} durationInFrames={30}>
  <MyComponent />
  {/* Inside MyComponent, useCurrentFrame() returns 0-29, not 60-89 */}
</Sequence>
```

## Nesting Compositions

```tsx
<AbsoluteFill>
  <Sequence width={COMPOSITION_WIDTH} height={COMPOSITION_HEIGHT}>
    <CompositionComponent />
  </Sequence>
</AbsoluteFill>
```
