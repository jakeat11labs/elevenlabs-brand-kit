---
name: can-decode
description: Check if a video can be decoded by the browser using Mediabunny
metadata:
  tags: decode, validation, video, audio, compatibility, browser
---

# Checking Browser Video Decode Compatibility

```tsx
import { ALL_FORMATS, Input, UrlSource } from "mediabunny";

export const canDecode = async (src: string): Promise<boolean> => {
  try {
    using input = new Input({
      formats: ALL_FORMATS,
      source: new UrlSource(src),
    });

    const format = await input.getFormat();
    const videoTrack = await input.getPrimaryVideoTrack();
    const audioTrack = await input.getPrimaryAudioTrack();

    if (videoTrack && !(await videoTrack.canDecode())) return false;
    if (audioTrack && !(await audioTrack.canDecode())) return false;

    return true;
  } catch {
    return false;
  }
};
```

## Usage

```tsx
const decodable = await canDecode("https://example.com/video.mp4");

if (decodable) {
  // Proceed with video playback
} else {
  // Fall back to alternative format or show error
}
```

## With Blob / File Upload

```tsx
import { BlobSource } from "mediabunny";

using input = new Input({
  formats: ALL_FORMATS,
  source: new BlobSource(file), // File from input or drag-drop
});
```
