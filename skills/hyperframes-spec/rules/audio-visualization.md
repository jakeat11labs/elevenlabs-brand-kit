---
name: audio-visualization
description: Audio visualization in Remotion - spectrum bars, waveforms, bass-reactive effects
metadata:
  tags: audio, visualization, spectrum, waveform, bass
---

# Audio Visualization

Uses `@remotion/media-utils`. Install: `npx remotion add @remotion/media-utils`

## Loading Audio Data

Use `useWindowedAudioData()` to efficiently load audio in manageable windows:

```tsx
import { useWindowedAudioData } from "@remotion/media-utils";
import { staticFile, useCurrentFrame } from "remotion";

const frame = useCurrentFrame();
const audioData = useWindowedAudioData({ src: staticFile("audio.mp3"), frame, fps: 30 });
```

## Spectrum Bar Visualization

```tsx
import { visualizeAudio } from "@remotion/media-utils";

const bars = visualizeAudio({
  audioData,
  frame,
  fps: 30,
  numberOfSamples: 64,   // Must be power of 2
  optimizeFor: "speed",  // Recommended for Lambda
});
// bars: array of 0–1 values (left=bass, right=treble)
```

## Waveform Display

```tsx
import { visualizeAudioWaveform, createSmoothSvgPath } from "@remotion/media-utils";

const waveform = visualizeAudioWaveform({ audioData, frame, fps: 30 });
const path = createSmoothSvgPath(waveform);

<svg><path d={path} stroke="white" fill="none" /></svg>
```

## Bass Reactivity

```tsx
const bars = visualizeAudio({ audioData, frame, fps: 30, numberOfSamples: 64 });
const bassIntensity = bars.slice(0, 4).reduce((a, b) => a + b, 0) / 4;
const scale = 1 + bassIntensity * 0.3;
```

## Volume-Based Data

```tsx
import { getWaveformPortion } from "@remotion/media-utils";

const waveform = getWaveformPortion({ audioData, frame, fps: 30 });
// Returns array of { amplitude: 0-1 } objects
```

## Important

Pass `frame` from parent components rather than calling `useCurrentFrame()` inside children — avoids visualization discontinuities within `<Sequence>` components.

Apply logarithmic scaling (dB) to balance bass-heavy visuals:
```tsx
const dB = bars.map(v => 20 * Math.log10(Math.max(v, 0.001)));
```
