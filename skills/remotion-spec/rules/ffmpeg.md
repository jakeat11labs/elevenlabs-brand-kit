---
name: ffmpeg
description: Using FFmpeg and FFprobe in Remotion
metadata:
  tags: ffmpeg, ffprobe, video, trimming
---

# FFmpeg in Remotion

`ffmpeg` and `ffprobe` do not need to be installed. Available via:

```bash
bunx remotion ffmpeg -i input.mp4 output.mp3
bunx remotion ffprobe input.mp4
```

## Trimming Videos

Two options:

**Option 1: FFmpeg CLI** — must re-encode to avoid frozen frames at start:

```bash
# Re-encodes from the exact frame
bunx remotion ffmpeg -ss 00:00:05 -i public/input.mp4 -to 00:00:10 -c:v libx264 -c:a aac public/output.mp4
```

**Option 2: Video component props** — non-destructive, changeable at any time:

```tsx
import { Video } from "@remotion/media";

const { fps } = useVideoConfig();

<Video
  src={staticFile("video.mp4")}
  trimBefore={5 * fps}
  trimAfter={10 * fps}
/>
```
