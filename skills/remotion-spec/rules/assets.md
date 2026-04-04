---
name: assets
description: Importing images, videos, audio, and fonts into Remotion
metadata:
  tags: assets, staticFile, public, images, videos, audio, fonts
---

# Assets

## Asset Storage

Place all assets in the `public/` folder at your project root.

## Required: staticFile()

Always use `staticFile()` to reference files from the `public/` folder:

```tsx
import { staticFile } from "remotion";
import { Img } from "remotion";
import { Video } from "@remotion/media";
import { Audio } from "@remotion/media";

// Images
<Img src={staticFile("photo.png")} />

// Videos
<Video src={staticFile("clip.mp4")} />

// Audio
<Audio src={staticFile("music.mp3")} />
```

`staticFile()` returns an encoded URL that works correctly when deploying to subdirectories.

## Remote Assets

External URLs can be used directly without `staticFile()`:

```tsx
<Img src="https://example.com/image.png" />
<Video src="https://example.com/video.mp4" />
```

Remotion components automatically ensure assets are fully loaded before rendering.
