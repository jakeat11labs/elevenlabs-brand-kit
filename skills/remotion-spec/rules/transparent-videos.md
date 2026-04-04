---
name: transparent-videos
description: Rendering out a video with transparency
metadata:
  tags: transparency, alpha, prores, webm, vp9
---

# Transparent Videos

## Transparent ProRes (for video editing software)

CLI:
```bash
npx remotion render MyComp --image-format=png --pixel-format=yuva444p10le --codec=prores --prores-profile=4444
```

Or via `calculateMetadata`:
```tsx
calculateMetadata={async () => ({
  defaultCodec: "prores",
  props: {
    pixelFormat: "yuva444p10le",
    imageFormat: "png",
    proresProfile: "4444",
  },
})}
```

## Transparent WebM / VP9 (for browser playback)

CLI:
```bash
npx remotion render MyComp --image-format=png --pixel-format=yuva420p --codec=vp9
```

Or via `calculateMetadata`:
```tsx
calculateMetadata={async () => ({
  defaultCodec: "vp9",
  props: {
    pixelFormat: "yuva420p",
    imageFormat: "png",
  },
})}
```

## Important

Your composition background must be transparent (no `<AbsoluteFill>` with a background color) for transparency to work in the output.
