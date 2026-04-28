# Chladni Patterns for Remotion

"Chladni patterns emerge when sound vibrations form visible shapes, turning sound into something we can see. They highlight our deep connection to voice and audio and are a core element of ElevenLabs' visual language."

## Available Chladni Assets

### Pre-Made Backgrounds (`brand-assets/backgrounds/chladni-closeup/`, 15 images)

These are saturated, dreamlike photographs that work as Hero scene backgrounds. They're not Chladni patterns themselves, but they follow the brand's image philosophy (atmospheric, blur-ready, sound metaphor). Use as Hero mode backgrounds with the standard compositing pipeline (image > blur 25 > noise 70% overlay).

| File | Description |
|------|-------------|
| `closeup-01-red-rock-blue-sky.jpg` | Red rock landscape, blue sky |
| `closeup-02-blue-coral-splash.jpg` | Blue coral splash |
| `closeup-03-blue-mountain-orange.jpg` | Blue mountain with orange accents |
| `closeup-04-blue-peach-dunes.jpg` | Blue-peach dune landscape |
| `closeup-05-green-leaves.jpg` | Green leaves close-up |
| `closeup-06-warm-abstract.jpg` | Warm abstract tones |
| `closeup-07-pink-teal-blend.jpg` | Pink and teal blend |
| `closeup-08-dreamy-clouds.jpg` | Dreamy cloud formation |
| `closeup-09-water-splash-blue.jpg` | Blue water splash |
| `closeup-10-mountains-motion.jpg` | Mountains with motion blur |
| `closeup-11-flowers-blur-warm.jpg` | Warm blurred flowers |
| `closeup-12-flowers-purple-pink.jpg` | Purple-pink flowers |
| `closeup-13-blue-purple-portrait.jpg` | Blue-purple portrait atmosphere |
| `closeup-14-blue-red-portrait.jpg` | Blue-red portrait atmosphere |
| `closeup-15-landscape-green.jpg` | Green landscape |

### Chladni Pattern Tiles (`brand-assets/backgrounds/general/`, 3 images)

Actual Chladni pattern images. Can be used as overlays on Hero scenes or as standalone backgrounds.

| File | Description |
|------|-------------|
| `image38-chladni-pattern.jpg` | Chladni pattern — concentric shapes |
| `image42-chladni-pattern.jpg` | Chladni pattern — radial lines |
| `image50-chladni-pattern.jpg` | Chladni pattern — organic shapes |

### Chladni Cover Banners (bundled `assets/covers/`, 4 images)

| File | Dimensions | Description |
|------|-----------|-------------|
| `Cover0.jpg` | 1500x500 or 1584x396 | Chladni pattern banner |
| `Cover1.jpg` | 1500x500 or 1584x396 | Chladni pattern banner |
| `Cover2.jpg` | 1500x500 or 1584x396 | Chladni pattern banner |
| `Cover3.jpg` | 1500x500 or 1584x396 | Chladni pattern banner |

Use as overlays at low opacity (0.03-0.08) on title/outro scenes, or as background textures.

## SVG Component (Code-Generated)

Ready-to-use SVG component that generates Chladni-inspired patterns for animated overlays. Use this when you need animated Chladni effects rather than static images.

```tsx
import { useCurrentFrame, useVideoConfig, interpolate } from "remotion";

// Chladni-inspired SVG pattern — signature ElevenLabs visual
export const ChladniPattern: React.FC<{
  opacity?: number;
  scale?: number;
  color?: string;
  position?: "right" | "left" | "center";
}> = ({
  opacity = 0.05,
  scale = 1,
  color = "#999999",
  position = "right",
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Very slow rotation — subtle movement
  const rotation = interpolate(frame, [0, 30 * fps], [0, 360], {
    extrapolateRight: "extend",
  });

  const positionStyles: Record<string, React.CSSProperties> = {
    right: { right: -200, top: -100 },
    left: { left: -200, top: -100 },
    center: { left: "50%", top: "50%", marginLeft: -400 * scale, marginTop: -400 * scale },
  };

  return (
    <svg
      viewBox="0 0 400 400"
      style={{
        position: "absolute",
        width: 800 * scale,
        height: 800 * scale,
        opacity,
        transform: `rotate(${rotation}deg)`,
        ...positionStyles[position],
      }}
    >
      {/* Concentric ellipses — organic Chladni shapes */}
      {[1, 0.75, 0.5, 0.3].map((s, i) => (
        <ellipse
          key={`e-${i}`}
          cx={200}
          cy={200}
          rx={180 * s}
          ry={160 * s}
          fill="none"
          stroke={color}
          strokeWidth={1}
          transform={`rotate(${i * 22.5}, 200, 200)`}
        />
      ))}

      {/* Secondary ellipses rotated 45deg */}
      {[0.9, 0.65, 0.4].map((s, i) => (
        <ellipse
          key={`e2-${i}`}
          cx={200}
          cy={200}
          rx={170 * s}
          ry={150 * s}
          fill="none"
          stroke={color}
          strokeWidth={0.5}
          transform={`rotate(${45 + i * 22.5}, 200, 200)`}
        />
      ))}

      {/* Radial lines */}
      {Array.from({ length: 12 }).map((_, i) => (
        <line
          key={`line-${i}`}
          x1={200}
          y1={200}
          x2={200 + 180 * Math.cos((i * Math.PI) / 6)}
          y2={200 + 180 * Math.sin((i * Math.PI) / 6)}
          stroke={color}
          strokeWidth={0.3}
        />
      ))}
    </svg>
  );
};
```

## Usage Guidelines

### On Hero Scenes
- Use Chladni pattern tiles or covers as **overlays** over the blurred background image
- Opacity: 0.03-0.08 — subtle texture, not a focal element
- Can use the SVG component for animated overlays with slow rotation
- Color: use WHITE or a light tone so it blends with the atmospheric background
- "Sharp Chladni shapes over blurred imagery emphasizes focus, clarity, and the precision of our technology"

### On Content Scenes
- Chladni patterns are optional on Content scenes
- If used, keep monochrome: MID_GRAY (#999999) or LIGHT_GRAY (#E0DEDC)
- Opacity: 0.03-0.06 maximum — background texture, not a visual element
- Best on title-adjacent Content scenes where a touch of brand identity helps the transition

### As Masks (Advanced)
- Chladni shapes can serve as masks over background images
- "These shapes are most often used in combination with photography, serving as masks that overlay images to create distinctive compositions"
- Use for special moments — concept reveals, methodology scenes

### Animation
- **Very slow rotation only** — one full rotation per 30 seconds
- No bouncing, scaling, or color changes
- The pattern should feel organic and alive, not mechanical
- Placement: typically right-aligned, partially off-screen. Center for outro scenes.

### Combining with Noise
- Chladni patterns layer well with the noise texture
- Compositing order: background image > blur > Chladni overlay > noise > content
- The noise adds grain that makes the Chladni feel more organic
