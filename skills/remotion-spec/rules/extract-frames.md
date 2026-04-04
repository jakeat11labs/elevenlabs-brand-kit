---
name: extract-frames
description: Extract frames from videos at specific timestamps using Mediabunny
metadata:
  tags: frames, extract, video, thumbnail, filmstrip, canvas
---

# Extracting Frames from Videos

## The extractFrames() Function

```tsx
import { ALL_FORMATS, Input, UrlSource, VideoSample, VideoSampleSink } from "mediabunny";

export async function extractFrames({
  src,
  timestampsInSeconds,
  onVideoSample,
  signal,
}: {
  src: string;
  timestampsInSeconds: number[] | ((opts: { track: { width: number; height: number }; container: string; durationInSeconds: number | null }) => Promise<number[]> | number[]);
  onVideoSample: (sample: VideoSample) => void;
  signal?: AbortSignal;
}): Promise<void> {
  using input = new Input({ formats: ALL_FORMATS, source: new UrlSource(src) });

  const [durationInSeconds, format, videoTrack] = await Promise.all([
    input.computeDuration(),
    input.getFormat(),
    input.getPrimaryVideoTrack(),
  ]);

  if (!videoTrack) throw new Error("No video track found");
  if (signal?.aborted) throw new Error("Aborted");

  const timestamps =
    typeof timestampsInSeconds === "function"
      ? await timestampsInSeconds({
          track: { width: videoTrack.displayWidth, height: videoTrack.displayHeight },
          container: format.name,
          durationInSeconds,
        })
      : timestampsInSeconds;

  if (timestamps.length === 0) return;

  const sink = new VideoSampleSink(videoTrack);

  for await (using videoSample of sink.samplesAtTimestamps(timestamps)) {
    if (signal?.aborted) break;
    if (!videoSample) continue;
    onVideoSample(videoSample);
  }
}
```

## Basic Usage

```tsx
await extractFrames({
  src: "https://example.com/video.mp4",
  timestampsInSeconds: [0, 1, 2, 3, 4],
  onVideoSample: (sample) => {
    const canvas = document.createElement("canvas");
    canvas.width = sample.displayWidth;
    canvas.height = sample.displayHeight;
    const ctx = canvas.getContext("2d");
    sample.draw(ctx!, 0, 0);
  },
});
```

## Filmstrip

```tsx
await extractFrames({
  src: "https://example.com/video.mp4",
  timestampsInSeconds: async ({ track, durationInSeconds }) => {
    const aspectRatio = track.width / track.height;
    const frameCount = Math.ceil(500 / (80 * aspectRatio));
    return Array.from({ length: frameCount }, (_, i) =>
      (durationInSeconds! / frameCount) * (i + 0.5)
    );
  },
  onVideoSample: (sample) => { /* draw to canvas */ },
});
```

## Cancellation

```tsx
const controller = new AbortController();
setTimeout(() => controller.abort(), 5000);

await extractFrames({
  src: "https://example.com/video.mp4",
  timestampsInSeconds: [0, 1, 2],
  onVideoSample: (sample) => { /* draw */ },
  signal: controller.signal,
});
```
