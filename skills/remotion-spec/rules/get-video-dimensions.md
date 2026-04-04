---
name: get-video-dimensions
description: Getting the width and height of a video file with Mediabunny
metadata:
  tags: dimensions, width, height, resolution, video
---

# Getting Video Dimensions

Uses Mediabunny. Works in browser, Node.js, and Bun.

```tsx
import { Input, ALL_FORMATS, UrlSource } from "mediabunny";

export const getVideoDimensions = async (src: string) => {
  const input = new Input({
    formats: ALL_FORMATS,
    source: new UrlSource(src, {
      getRetryDelay: () => null,
    }),
  });

  const videoTrack = await input.getPrimaryVideoTrack();
  if (!videoTrack) {
    throw new Error("No video track found");
  }

  return {
    width: videoTrack.displayWidth,
    height: videoTrack.displayHeight,
  };
};
```

## Usage

```tsx
const { width, height } = await getVideoDimensions("https://example.com/video.mp4");
// e.g. { width: 1920, height: 1080 }

// With Remotion local files:
import { staticFile } from "remotion";
const dims = await getVideoDimensions(staticFile("video.mp4"));
```

## Node.js / Bun

Replace `UrlSource` with `FileSource`:

```tsx
import { Input, ALL_FORMATS, FileSource } from "mediabunny";

const input = new Input({ formats: ALL_FORMATS, source: new FileSource(file) });
const videoTrack = await input.getPrimaryVideoTrack();
```
