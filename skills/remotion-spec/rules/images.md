---
name: images
description: Embedding images in Remotion using the Img component
metadata:
  tags: images, Img, staticFile, remote
---

# Images in Remotion

Use the `<Img>` component — NOT HTML `<img>`, Next.js `<Image>`, or CSS background images. The Remotion component ensures images are loaded before rendering.

```tsx
import { Img, staticFile } from "remotion";

// Local image
<Img src={staticFile("logo.png")} />

// Remote image (CORS must be enabled)
<Img src="https://example.com/image.png" />
```

## Styling

```tsx
<Img
  src={staticFile("photo.jpg")}
  style={{ width: "100%", height: "100%", objectFit: "cover" }}
/>
```

## Dynamic Paths

```tsx
// Frame-by-frame sequences
const frame = useCurrentFrame();
<Img src={staticFile(`frame-${String(frame).padStart(4, "0")}.png`)} />

// Conditional
<Img src={staticFile(isDark ? "logo-white.png" : "logo-black.png")} />
```

## Getting Image Dimensions

```tsx
import { getImageDimensions } from "@remotion/media-utils";

const { width, height } = await getImageDimensions(staticFile("photo.jpg"));
```

## Animated GIFs

Use `<Gif>` from `@remotion/gif` for animated GIFs — not `<Img>`.
