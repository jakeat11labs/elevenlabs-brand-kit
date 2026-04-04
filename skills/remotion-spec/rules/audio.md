---
name: audio
description: Using audio and sound in Remotion - importing, trimming, volume, speed, pitch
metadata:
  tags: audio, media, trim, volume, speed, loop, pitch, mute, sound, sfx
---

# Using Audio in Remotion

## Prerequisites

Install `@remotion/media`: `npx remotion add @remotion/media`

## Importing Audio

```tsx
import { Audio } from "@remotion/media";
import { staticFile } from "remotion";

export const MyComposition = () => {
  return <Audio src={staticFile("audio.mp3")} />;
};
```

Remote URLs are also supported. Multiple `<Audio>` components can be layered.

## Trimming

Values are in frames:

```tsx
const { fps } = useVideoConfig();

<Audio
  src={staticFile("audio.mp3")}
  trimBefore={2 * fps}  // Skip first 2 seconds
  trimAfter={10 * fps}  // End at 10 second mark
/>
```

## Delaying

Wrap in `<Sequence>` to delay playback:

```tsx
<Sequence from={1 * fps}>
  <Audio src={staticFile("audio.mp3")} />
</Sequence>
```

## Volume

Static volume (0–1):
```tsx
<Audio src={staticFile("audio.mp3")} volume={0.5} />
```

Dynamic volume (fade-in):
```tsx
const { fps } = useVideoConfig();

<Audio
  src={staticFile("audio.mp3")}
  volume={(f) =>
    interpolate(f, [0, 1 * fps], [0, 1], { extrapolateRight: "clamp" })
  }
/>
```

Note: `f` starts at 0 when audio begins, not at composition frame.

## Muting

```tsx
const frame = useCurrentFrame();
const { fps } = useVideoConfig();

<Audio
  src={staticFile("audio.mp3")}
  muted={frame >= 2 * fps && frame <= 4 * fps}
/>
```

## Speed

```tsx
<Audio src={staticFile("audio.mp3")} playbackRate={2} />   // 2x speed
<Audio src={staticFile("audio.mp3")} playbackRate={0.5} /> // Half speed
```

Reverse playback is not supported.

## Looping

```tsx
<Audio src={staticFile("audio.mp3")} loop />

// Control frame count behavior on loop:
<Audio
  src={staticFile("audio.mp3")}
  loop
  loopVolumeCurveBehavior="extend"  // "repeat" (default) or "extend"
  volume={(f) => interpolate(f, [0, 300], [1, 0])}
/>
```

## Pitch

```tsx
<Audio src={staticFile("audio.mp3")} toneFrequency={1.5} />  // Higher pitch
<Audio src={staticFile("audio.mp3")} toneFrequency={0.8} />  // Lower pitch
```

Range: 0.01–2. Only works during server-side rendering, not in Studio preview or Player.
