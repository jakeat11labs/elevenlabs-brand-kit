---
name: parameters
description: Make a video parametrizable by adding a Zod schema
metadata:
  tags: parameters, zod, schema
---

# Parametrizable Videos with Zod

## Install Zod

```bash
npm i zod       # package-lock.json
bun i zod       # bun.lockb
yarn add zod    # yarn.lock
pnpm i zod      # pnpm-lock.yaml
```

## Define Schema + Component

```tsx
// src/MyComposition.tsx
import { z } from "zod";

export const MyCompositionSchema = z.object({
  title: z.string(),
});

const MyComponent: React.FC<z.infer<typeof MyCompositionSchema>> = (props) => {
  return <div><h1>{props.title}</h1></div>;
};
```

## Register in Root

```tsx
// src/Root.tsx
import { Composition } from "remotion";
import { MyComponent, MyCompositionSchema } from "./MyComposition";

export const RemotionRoot = () => (
  <Composition
    id="MyComposition"
    component={MyComponent}
    durationInFrames={100}
    fps={30}
    width={1080}
    height={1080}
    defaultProps={{ title: "Hello World" }}
    schema={MyCompositionSchema}
  />
);
```

Users can now edit parameters visually in the Studio sidebar.

The top-level type must be `z.object()` — component props are always objects.

## Color Picker

```bash
npx remotion add @remotion/zod-types
```

```tsx
import { zColor } from "@remotion/zod-types";

export const MySchema = z.object({
  color: zColor(),
});
```
