---
name: gifs
description: Displaying GIFs synchronized with Remotion's timeline
metadata:
  tags: gif, animated, apng, webp, avif
---

# GIFs and Animated Images

## AnimatedImage (Recommended)

Supports GIF, APNG, AVIF, WebP. Syncs with Remotion's timeline:

```tsx
import { AnimatedImage } from "remotion";
import { staticFile } from "remotion";

<AnimatedImage
  src={staticFile("animation.gif")}
  fit="contain"          // "fill" | "contain" | "cover"
  playbackRate={1}       // Speed multiplier
  loop="loop"            // "loop" | "pause-after-finish" | "clear-after-finish"
  width={400}
  height={300}
  style={{ borderRadius: 8 }}
/>
```

## Getting GIF Duration

```tsx
import { getGifDurationInSeconds } from "@remotion/gif";
import { staticFile } from "remotion";

// In calculateMetadata:
const duration = await getGifDurationInSeconds(staticFile("animation.gif"));
const durationInFrames = Math.round(duration * fps);
```

## Fallback: @remotion/gif

For GIF-only support with broader browser compatibility:

```tsx
import { Gif } from "@remotion/gif";
import { staticFile } from "remotion";

<Gif src={staticFile("animation.gif")} width={400} height={300} fit="contain" />
```
