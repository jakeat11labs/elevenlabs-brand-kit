# Animation Patterns

## Core Animation Rule

**Every animation in this project uses the same spring configuration:**

```tsx
spring({ frame, fps, delay: [varies], config: { damping: 200 } })
```

Damping 200 = snappy settle, ~0.5-0.7 seconds to complete. No bounce. Confident.

**Never use:**
- CSS transitions or animations (won't render)
- Tailwind animation classes (won't render)
- `useFrame()` from React Three Fiber (causes flickering)
- Raw frame numbers — always use `seconds * fps`

## Standard Setup

Every component starts with:

```tsx
const frame = useCurrentFrame();
const { fps } = useVideoConfig();
```

## Entrance Animations

| Element | Transform | Delay Pattern |
|---------|----------|---------------|
| Logo/icon | translateY 15px → 0 | 5-10 frames |
| Section label | translateY 10px → 0 | 0-12 frames |
| Headline | translateY 12-25px → 0 | 12-24 frames |
| Accent line | width 0 → target | 6-28 frames |
| Subtitle/body | translateY 10px → 0 | 25-34 frames |
| Cards/panels | translateY 40px → 0 | 10 + i*15 frames |
| List items | translateX 15px → 0 | 8 + i*4 frames |
| Badges | scale 0 → 1 | 60 frames |

## Stagger Pattern (Cards)

Cards enter from below, staggered 15 frames apart:

```tsx
{items.map((item, i) => {
  const cardProgress = spring({
    frame,
    fps,
    delay: 10 + i * 15,
    config: { damping: 200 },
  });

  return (
    <div style={{
      opacity: interpolate(cardProgress, [0, 1], [0, 1]),
      transform: `translateY(${interpolate(cardProgress, [0, 1], [40, 0])}px)`,
    }}>
      {/* card content */}
    </div>
  );
})}
```

## Stagger Pattern (List Items)

List items enter from the side, staggered 4 frames apart:

```tsx
{items.map((item, i) => {
  const itemProgress = spring({
    frame,
    fps,
    delay: 8 + i * 4,
    config: { damping: 200 },
  });

  return (
    <div style={{
      opacity: interpolate(itemProgress, [0, 1], [0, 1]),
      transform: `translateX(${interpolate(itemProgress, [0, 1], [15, 0])}px)`,
    }}>
      {/* item content */}
    </div>
  );
})}
```

## Combining Spring with Interpolation

When you need to map a spring (0→1) to a specific range:

```tsx
const progress = spring({ frame, fps, config: { damping: 200 } });
const translateY = interpolate(progress, [0, 1], [40, 0]);
const opacity = interpolate(progress, [0, 1], [0, 1]);
```

## Line/Bar Animations

Accent lines animate their width:
```tsx
const lineProgress = spring({ frame, fps, delay: 6, config: { damping: 200 } });

// Percentage-based line
<div style={{ width: `${100 * lineProgress}%`, height: 1, backgroundColor: BRAND.LIGHT_GRAY }} />

// Fixed-width accent line
<div style={{ width: 60 * lineProgress, height: 2, backgroundColor: BRAND.GRAPHITE }} />

// Bottom bar with scaleX
<div style={{
  transform: `scaleX(${barProgress})`,
  transformOrigin: "left",
  height: 3,
  backgroundColor: BRAND.GRAPHITE,
}} />
```

## Vertical Divider Animation

```tsx
const lineProgress = spring({ frame, fps, delay: 28, config: { damping: 200 } });

<div style={{
  width: 1,
  backgroundColor: BRAND.LIGHT_GRAY,
  opacity: interpolate(lineProgress, [0, 1], [0, 0.4]),
  transformOrigin: "top",
  transform: `scaleY(${lineProgress})`,
}} />
```

## Cover Image Treatment

For split title cards with cover images:

```tsx
const coverProgress = spring({ frame, fps, config: { damping: 200 } });

<Img
  src={staticFile("covers/Cover1.jpg")}
  style={{
    width: "100%",
    height: "100%",
    objectFit: "cover",
    opacity: interpolate(coverProgress, [0, 1], [0, 0.25]),
    filter: "grayscale(100%) contrast(1.1)",
    transform: `scale(${interpolate(coverProgress, [0, 1], [1.05, 1])})`,
  }}
/>
```

## Scene Transitions

### V1 Transitions
```tsx
const TRANSITION_FRAMES = 20;  // 0.67s
```

### V2 Transitions (use this for new compositions)
```tsx
const TRANSITION_FRAMES = 45;  // 1.5s crossfade
```

Between every scene, always use fade with linear timing:

```tsx
import { TransitionSeries, linearTiming } from "@remotion/transitions";
import { fade } from "@remotion/transitions/fade";

<TransitionSeries.Transition
  presentation={fade()}
  timing={linearTiming({ durationInFrames: TRANSITION_FRAMES })}
/>
```

No slides, wipes, or flips. Fade only.

## V2 Animation Timing

V2 scenes add entrance delay and exit fade to prevent content overlap during crossfades.

### ENTER_DELAY
All entrance springs are delayed by 35 frames so content doesn't appear while the previous scene is still fading:
```tsx
const ENTER_DELAY = 35;

const headerProgress = spring({ frame, fps, delay: ENTER_DELAY, config: { damping: 200 } });
const lineProgress = spring({ frame, fps, delay: ENTER_DELAY + 6, config: { damping: 200 } });
// Cards stagger: delay: ENTER_DELAY + 10 + i * 15
```

### Exit Fade (exitOpacity)
Content fades out before the scene-level crossfade, leaving just the gradient visible:
```tsx
const { fps, durationInFrames } = useVideoConfig();

const exitOpacity = interpolate(
  frame,
  [durationInFrames - 40, durationInFrames - 10],
  [1, 0],
  { extrapolateLeft: "clamp", extrapolateRight: "clamp" }
);
```
Apply `exitOpacity` to the content wrapper (NOT the GradientBackground). Also wrap IIWatermark:
```tsx
<div style={{ opacity: exitOpacity }}><IIWatermark color="white" /></div>
```

### V2 Timing Flow
For a typical 8-second (240 frame) scene:
| Frames | What Happens |
|--------|-------------|
| 0-35 | Entrance delay — just gradient visible |
| 35-90 | Cards spring in (staggered) |
| 90-200 | **Hold** — content fully on screen, readable |
| 200-230 | Exit fade — content fades out over 30 frames |
| 230-240 | Just gradient — breathing room before crossfade |

### Minimum Scene Duration
8 seconds (240 frames) minimum for v2 scenes to allow entrance + hold + exit.

## Duration Calculation

With transitions overlapping, actual total duration is:

```
total = sum(sceneDurations) - (numTransitions × TRANSITION_FRAMES / FPS)
```

For a 13-scene composition with 12 transitions at 20 frames each:
- Scene sum: 102 seconds
- Overlap: 12 × 20/30 = 8 seconds
- Actual: ~94 seconds of video
