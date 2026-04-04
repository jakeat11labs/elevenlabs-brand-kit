---
name: videos
description: Embedding videos in Remotion - trimming, volume, speed, looping, pitch
metadata:
  tags: video, media, trim, volume, speed, loop, pitch, mute
---

# Using Videos in Remotion

## Prerequisites

Install `@remotion/media`: `npx remotion add @remotion/media`

```tsx
import { Video } from "@remotion/media";
import { staticFile } from "remotion";

<Video src={staticFile("clip.mp4")} />
<Video src="https://example.com/video.mp4" />
```

## Trimming

```tsx
const { fps } = useVideoConfig();

<Video
  src={staticFile("video.mp4")}
  trimBefore={2 * fps}   // Skip first 2 seconds
  trimAfter={10 * fps}   // End at 10 second mark
/>
```

## Delaying

```tsx
<Sequence from={1 * fps}>
  <Video src={staticFile("video.mp4")} />
</Sequence>
```

## Sizing and Position

```tsx
<Video
  src={staticFile("video.mp4")}
  style={{ width: "100%", height: "100%", objectFit: "cover" }}
/>
```

## Volume

```tsx
// Static
<Video src={staticFile("video.mp4")} volume={0.5} />

// Dynamic
<Video
  src={staticFile("video.mp4")}
  volume={(f) =>
    interpolate(f, [0, 1 * fps], [0, 1], { extrapolateRight: "clamp" })
  }
/>

// Muted
<Video src={staticFile("video.mp4")} muted />
```

## Speed

```tsx
<Video src={staticFile("video.mp4")} playbackRate={2} />   // 2x
<Video src={staticFile("video.mp4")} playbackRate={0.5} /> // Half speed
```

Reverse playback is not supported.

## Looping

```tsx
<Video src={staticFile("video.mp4")} loop />

// Control volume curve behavior across loops:
<Video
  src={staticFile("video.mp4")}
  loop
  loopVolumeCurveBehavior="extend"
/>
```

## Pitch

```tsx
<Video src={staticFile("video.mp4")} toneFrequency={1.5} />
```

Range: 0.01–2. Only works during server-side rendering.
