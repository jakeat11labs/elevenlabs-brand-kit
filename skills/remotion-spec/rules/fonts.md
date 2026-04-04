---
name: fonts
description: Loading Google Fonts and local fonts in Remotion
metadata:
  tags: fonts, google-fonts, typography
---

# Fonts in Remotion

## Google Fonts (@remotion/google-fonts)

Install: `npx remotion add @remotion/google-fonts`

Type-safe, automatically blocks rendering until fonts load:

```tsx
import { loadFont } from "@remotion/google-fonts/Lobster";

const { fontFamily } = loadFont();

export const MyComp = () => (
  <div style={{ fontFamily }}>Hello World</div>
);
```

Specify only needed weights/subsets for optimization:

```tsx
const { fontFamily } = loadFont("normal", {
  weights: ["400", "700"],
  subsets: ["latin"],
});
```

Wait explicitly for font load:

```tsx
await loadFont().waitUntilDone();
```

## Local Fonts (@remotion/fonts)

Install: `npx remotion add @remotion/fonts`

```tsx
import { loadFont } from "@remotion/fonts";
import { staticFile } from "remotion";

// Single weight
await loadFont({
  family: "MyFont",
  url: staticFile("fonts/MyFont-Regular.woff2"),
});

// Multiple weights
await Promise.all([
  loadFont({ family: "MyFont", url: staticFile("fonts/MyFont-Regular.woff2"), weight: "400" }),
  loadFont({ family: "MyFont", url: staticFile("fonts/MyFont-Bold.woff2"), weight: "700" }),
]);
```

## Best Practice

Call `loadFont()` at the top level of your component or in a file imported early, to ensure fonts load before rendering.
