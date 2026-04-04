---
name: get-audio-duration
description: Getting the duration of an audio file in seconds with Mediabunny
metadata:
  tags: duration, audio, mediabunny
---

# Getting Audio Duration

Uses Mediabunny. Works in browser, Node.js, and Bun.

```tsx
import { Input, ALL_FORMATS, UrlSource } from "mediabunny";

export const getAudioDuration = async (src: string): Promise<number> => {
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
const duration = await getAudioDuration("https://example.com/audio.mp3");
// e.g. 180.5 (seconds)

// With Remotion local files:
import { staticFile } from "remotion";
const duration = await getAudioDuration(staticFile("audio.mp3"));
```

## Node.js / Bun

Replace `UrlSource` with `FileSource`:

```tsx
import { Input, ALL_FORMATS, FileSource } from "mediabunny";

const input = new Input({
  formats: ALL_FORMATS,
  source: new FileSource(file), // File from input element or drag-drop
});
const duration = await input.computeDuration();
```
