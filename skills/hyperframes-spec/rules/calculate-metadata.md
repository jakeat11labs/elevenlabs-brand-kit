---
name: calculate-metadata
description: Dynamically set composition duration, dimensions, and props
metadata:
  tags: calculateMetadata, duration, dimensions, async, props
---

# calculateMetadata

Dynamically sets duration, dimensions, and props before rendering on a `<Composition>`.

## Video Duration

```tsx
import { getVideoDuration } from "@remotion/media-utils";

calculateMetadata={async ({ props }) => {
  const duration = await getVideoDuration(props.src);
  return {
    durationInFrames: Math.round(duration * props.fps),
  };
}}
```

## Video Dimensions

```tsx
import { getVideoDimensions } from "@remotion/media-utils";

calculateMetadata={async ({ props }) => {
  const { width, height } = await getVideoDimensions(props.src);
  return { width, height };
}}
```

## Multiple Videos

```tsx
calculateMetadata={async ({ props }) => {
  const durations = await Promise.all(
    props.videos.map((src) => getVideoDuration(src))
  );
  const totalFrames = Math.round(durations.reduce((a, b) => a + b, 0) * fps);
  return { durationInFrames: totalFrames };
}}
```

## Props Transformation with External Data

```tsx
calculateMetadata={async ({ props, abortSignal }) => {
  const data = await fetch(`/api/video/${props.id}`, { signal: abortSignal }).then(r => r.json());
  return {
    props: { ...props, ...data },
    defaultOutName: `video-${props.id}.mp4`,
  };
}}
```

## Returnable Fields (all optional)

- `durationInFrames`, `width`, `height`, `fps`
- `props` (transformed)
- `defaultOutName`, `defaultCodec`
