---
name: maps
description: Add a map using Mapbox and animate it
metadata:
  tags: maps, mapbox, geospatial, animation
---

# Maps with Mapbox

## Prerequisites

```bash
npm install mapbox-gl @turf/turf @types/mapbox-gl
```

Store your token: `REMOTION_MAPBOX_TOKEN=pk.xxx` in `.env`

## Critical Settings

```tsx
const map = new mapboxgl.Map({
  container: mapContainerRef.current,
  style: "mapbox://styles/mapbox/standard",
  accessToken: process.env.REMOTION_MAPBOX_TOKEN,
  interactive: false,      // Disable user interaction
  fadeDuration: 0,         // Disable automatic transitions
});
```

The container must have explicit dimensions:

```tsx
<div
  ref={mapContainerRef}
  style={{ width: videoWidth, height: videoHeight, position: "absolute" }}
/>
```

## Loading Management

Use `delayRender`/`continueRender` to pause until map loads:

```tsx
import { delayRender, continueRender } from "remotion";

const handle = delayRender();

map.on("load", () => {
  continueRender(handle);
});
```

## Camera Animation

Move the camera along a route using Turf.js:

```tsx
import * as turf from "@turf/turf";

const route = turf.lineString(coordinates);
const routeLength = turf.length(route, { units: "kilometers" });

const frame = useCurrentFrame();
const { fps, durationInFrames } = useVideoConfig();
const progress = frame / durationInFrames;

const point = turf.along(route, routeLength * progress, { units: "kilometers" });
const [lng, lat] = point.geometry.coordinates;

map.jumpTo({ center: [lng, lat] });
```

Unless requested, do not jump between camera angles. Keep north pointing upward by default.

## Line Animation (Straight Lines)

For lines that appear straight on a map, use linear interpolation between coordinates rather than Turf's geodesic functions (which appear curved on Mercator projections):

```tsx
const progress = frame / durationInFrames;
const lng = startLng + (endLng - startLng) * progress;
const lat = startLat + (endLat - startLat) * progress;
```

## Key Constraints

- Do not add glow effects to lines unless specifically requested
- Do not add additional interpolated points unless requested
- Set all animation properties at every frame to prevent visual jumps
- Consider composition dimensions when sizing lines and text for legibility
