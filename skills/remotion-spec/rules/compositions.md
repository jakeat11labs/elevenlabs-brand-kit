---
name: compositions
description: Defining compositions, stills, folders, default props and dynamic metadata
metadata:
  tags: composition, still, folder, defaultProps, calculateMetadata
---

# Compositions

A `<Composition>` establishes video settings (dimensions, FPS, duration, component). Defined in `src/Root.tsx`.

```tsx
import { Composition } from "remotion";
import { MyComponent } from "./MyComposition";

export const RemotionRoot = () => {
  return (
    <Composition
      id="MyComposition"
      component={MyComponent}
      durationInFrames={150}
      fps={30}
      width={1920}
      height={1080}
      defaultProps={{ title: "Hello World" }}
    />
  );
};
```

## Default Props

Supply initial values using `defaultProps`. Supported: JSON-serializable types, `Date`, `Map`, `Set`, `staticFile()`.

## Folder Organization

```tsx
import { Folder } from "remotion";

<Folder name="my-folder">
  <Composition ... />
</Folder>
```

Folder names: letters, numbers, hyphens only.

## Stills

Single-frame images — no duration or FPS required:

```tsx
import { Still } from "remotion";

<Still id="Thumbnail" component={ThumbnailComponent} width={1280} height={720} />
```

## Dynamic Metadata

Use `calculateMetadata` to compute composition properties from external data:

```tsx
<Composition
  id="DynamicVideo"
  component={MyComponent}
  durationInFrames={150}
  fps={30}
  width={1920}
  height={1080}
  calculateMetadata={async ({ props }) => {
    const duration = await getVideoDuration(props.src);
    return { durationInFrames: Math.round(duration * 30) };
  }}
/>
```

## Type Safety

Use `type` (not `interface`) for component props to ensure proper type checking with `defaultProps`.
