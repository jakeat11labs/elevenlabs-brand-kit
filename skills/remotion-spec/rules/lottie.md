---
name: lottie
description: Embedding Lottie animations in Remotion
metadata:
  tags: lottie, animation, json
---

# Lottie in Remotion

Install: `npx remotion add @remotion/lottie`

## Basic Usage

```tsx
import { Lottie } from "@remotion/lottie";
import { useEffect, useState } from "react";
import { continueRender, delayRender, staticFile } from "remotion";

export const MyComp = () => {
  const [animationData, setAnimationData] = useState(null);
  const [handle] = useState(() => delayRender());

  useEffect(() => {
    fetch(staticFile("animation.json"))
      .then((res) => res.json())
      .then((data) => {
        setAnimationData(data);
        continueRender(handle);
      })
      .catch((err) => {
        console.error("Failed to load Lottie:", err);
        continueRender(handle);
      });
  }, [handle]);

  if (!animationData) return null;

  return (
    <Lottie
      animationData={animationData}
      style={{ width: 400, height: 400 }}
    />
  );
};
```

## Important

Wrap the loading process in `delayRender()` and `continueRender()` to ensure rendering pauses until the animation loads completely.
