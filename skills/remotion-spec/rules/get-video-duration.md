---
name: get-video-duration
description: Getting the duration of a video file in seconds with Mediabunny
metadata:
  tags: duration, video, mediabunny
---

# Getting Video Duration

Uses Mediabunny. Works in browser, Node.js, and Bun.

```tsx
import { Input, ALL_FORMATS, UrlSource } from "mediabunny";

export const getVideoDuration = async (src: string): Promise<number> => {
  const input = new Input({
    formats: ALL_FORMATS,
    source: new UrlSource(src, {
      getRetryDelay: () => null,
    }),
  });

  return await input.computeDuration();
};
```

## Usage

```tsx
const duration = await getVideoDuration("https://example.com/video.mp4");
// e.g. 10.5 (seconds)

// With Remotion local files:
import { staticFile } from "remotion";
const duration = await getVideoDuration(staticFile("video.mp4"));
```

## Node.js / Bun

Replace `UrlSource` with `FileSource` for File objects:

```tsx
import { Input, ALL_FORMATS, FileSource } from "mediabunny";

const input = new Input({
  formats: ALL_FORMATS,
  source: new FileSource(file), // File from input element or drag-drop
});
const duration = await input.computeDuration();
```
