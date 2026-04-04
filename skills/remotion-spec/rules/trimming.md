---
name: trimming
description: Trimming patterns for Remotion - cut the beginning or end of animations
metadata:
  tags: sequence, trim, clip, cut, offset
---

# Trimming Animations

## Trim the Beginning

Use a negative `from` value in `<Sequence>` to skip ahead in the animation:

```tsx
const { fps } = useVideoConfig();

// Starts 0.5s into the animation — useCurrentFrame() begins at 15
<Sequence from={-0.5 * fps}>
  <MyAnimation />
</Sequence>
```

## Trim the End

Use `durationInFrames` to unmount content after a specified duration:

```tsx
// Plays for 1.5 seconds then unmounts
<Sequence durationInFrames={1.5 * fps}>
  <MyAnimation />
</Sequence>
```

## Trim and Delay

Nest sequences to combine both effects:

```tsx
// Delayed by 30 frames, starts 15 frames into the animation
<Sequence from={30}>
  <Sequence from={-15}>
    <MyAnimation />
  </Sequence>
</Sequence>
```
