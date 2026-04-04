---
name: measuring-text
description: Measuring text dimensions, fitting text to containers, and checking overflow
metadata:
  tags: text, measure, layout, overflow, font-size
---

# Measuring Text

Install: `npx remotion add @remotion/layout-utils`

## Measure Text Dimensions

```tsx
import { measureText } from "@remotion/layout-utils";

const { width, height } = measureText({
  text: "Hello World",
  fontFamily: "KMR Waldenburg",
  fontSize: 48,
  fontWeight: "400",
});
```

Results are cached for duplicate calls.

## Fit Text to Width

```tsx
import { fitText } from "@remotion/layout-utils";

const { fontSize } = fitText({
  text: "Hello World",
  withinWidth: 800,
  fontFamily: "KMR Waldenburg",
  fontWeight: "700",
  maxFontSize: 100,  // optional cap
});
```

## Check Overflow

```tsx
import { fillTextBox } from "@remotion/layout-utils";

const result = fillTextBox({
  text: "Long text...",
  maxWidth: 600,
  maxLines: 3,
  fontFamily: "KMR Waldenburg",
  fontSize: 32,
});
// result.overflows: boolean
```

## Best Practices

1. **Load fonts first** — call measurement functions only after `waitUntilDone()`
2. **Validate early** — use `validateFontIsLoaded: true`
3. **Match properties** — use identical font properties in both measurement and rendering
4. **Use CSS `outline`** instead of `border` — borders affect text dimensions
