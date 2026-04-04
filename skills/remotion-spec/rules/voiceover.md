---
name: voiceover
description: Adding AI-generated voiceover to Remotion compositions using ElevenLabs TTS
metadata:
  tags: voiceover, tts, elevenlabs, audio, narration
---

# AI Voiceover with ElevenLabs

## Requirements

Requires `ELEVENLABS_API_KEY` environment variable. If not available, ask the user for their ElevenLabs API key. Do NOT fall back to alternative TTS services.

## Workflow

1. Generate audio files from text scripts using ElevenLabs API
2. Use `calculateMetadata` to measure audio lengths and set composition duration dynamically
3. Render audio files in Remotion components with `<Audio>`

## Step 1: Generate Audio

```tsx
import { writeFileSync } from "fs";

async function generateVoiceover(text: string, outputPath: string) {
  const response = await fetch(
    "https://api.elevenlabs.io/v1/text-to-speech/{voice_id}",
    {
      method: "POST",
      headers: {
        "xi-api-key": process.env.ELEVENLABS_API_KEY!,
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        text,
        model_id: "eleven_multilingual_v2",
        voice_settings: {
          stability: 0.5,
          similarity_boost: 0.75,
          style: 0,
        },
      }),
    }
  );

  const buffer = await response.arrayBuffer();
  writeFileSync(outputPath, Buffer.from(buffer));
  // Save to public/ directory for use with staticFile()
}
```

## Step 2: Dynamic Duration with calculateMetadata

```tsx
import { getAudioDuration } from "./utils/getAudioDuration";

const FPS = 30;

calculateMetadata={async ({ props }) => {
  const durations = await Promise.all(
    props.scenes.map((scene) =>
      getAudioDuration(staticFile(`audio/${scene.id}.mp3`))
    )
  );
  const totalSeconds = durations.reduce((a, b) => a + b, 0);

  // If using transitions, subtract overlap:
  // const overlapFrames = (props.scenes.length - 1) * TRANSITION_FRAMES;
  // return { durationInFrames: Math.round(totalSeconds * FPS) - overlapFrames };

  return { durationInFrames: Math.round(totalSeconds * FPS) };
}}
```

## Step 3: Render Audio

```tsx
import { Audio } from "@remotion/media";
import { staticFile } from "remotion";
import { Series } from "remotion";

<Series>
  {scenes.map((scene) => (
    <Series.Sequence key={scene.id} durationInFrames={scene.durationInFrames}>
      <Audio src={staticFile(`audio/${scene.id}.mp3`)} />
      <SceneVisuals scene={scene} />
    </Series.Sequence>
  ))}
</Series>
```
