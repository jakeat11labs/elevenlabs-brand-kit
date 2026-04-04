---
name: subtitles
description: Captions and subtitles in Remotion using @remotion/captions
metadata:
  tags: captions, subtitles, srt, transcription
---

# Captions and Subtitles

## Caption Type

All captions must use the `Caption` type from `@remotion/captions`:

```tsx
import type { Caption } from "@remotion/captions";

type Caption = {
  text: string;
  startMs: number;
  endMs: number;
  timestampMs?: number;
  confidence?: number;
};
```

## Three Main Workflows

### 1. Generating Captions

Transcribe video/audio files using a transcription service (e.g., ElevenLabs, Whisper). Output must be in JSON format using the `Caption` type.

### 2. Displaying Captions

```tsx
import { Caption } from "@remotion/captions";
import { useCurrentFrame, useVideoConfig } from "remotion";

const frame = useCurrentFrame();
const { fps } = useVideoConfig();
const currentTimeMs = (frame / fps) * 1000;

const activeCaption = captions.find(
  (c) => currentTimeMs >= c.startMs && currentTimeMs <= c.endMs
);

return <div>{activeCaption?.text}</div>;
```

### 3. Importing from SRT

```tsx
import { parseSrt } from "@remotion/captions";

const srtContent = `1
00:00:01,000 --> 00:00:03,000
Hello World`;

const captions = parseSrt({ input: srtContent });
```
