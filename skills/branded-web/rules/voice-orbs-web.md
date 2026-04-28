# Voice Orbs for Web

How to use ElevenLabs voice orbs in web projects.

## Basic CSS Compositing

Voice orbs ship as JPGs on black backgrounds. Use `mix-blend-mode: screen` to knock out the black:

```html
<div class="orb-container">
  <img
    src="/brand-assets/voice-orbs/orb-teal-on-black.jpg"
    alt=""
    class="voice-orb"
  />
</div>
```

```css
.orb-container {
  position: relative;
  background: #1e1916; /* or any dark background */
}

.voice-orb {
  mix-blend-mode: screen;
  width: 200px;
  height: auto;
}
```

**Important:** `mix-blend-mode: screen` only works against dark backgrounds. On light backgrounds, the orb's black area won't disappear properly. For light backgrounds, use the orb as a decorative element in a dark container.

## Available Orbs

8 color variants, all 1174x1038px JPGs:

| File | Color |
|------|-------|
| `orb-teal-on-black.jpg` | Teal |
| `orb-purple-blue-on-black.jpg` | Purple-blue |
| `orb-pink-on-black.jpg` | Pink |
| `orb-coral-on-black.jpg` | Coral |
| `orb-green-on-black.jpg` | Green |
| `orb-gold-on-black.jpg` | Gold |
| `orb-hotpink-on-black.jpg` | Hot pink |
| `orb-lavender-on-black.jpg` | Lavender |

## Animated Orb (CSS)

Simple CSS animations for decorative orbs:

```css
@keyframes orb-float {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-10px) scale(1.02); }
}

@keyframes orb-pulse {
  0%, 100% { opacity: 0.8; }
  50% { opacity: 1; }
}

.voice-orb-animated {
  mix-blend-mode: screen;
  animation:
    orb-float 6s ease-in-out infinite,
    orb-pulse 4s ease-in-out infinite;
}
```

## Three.js / React Three Fiber

For an interactive 3D orb (as used in the existing product demo implementation), use a shader-based approach with `@react-three/fiber`:

```tsx
import { Canvas } from '@react-three/fiber';
import { useTexture } from '@react-three/drei';

function Orb({ color = '#00d4aa' }) {
  const noiseTexture = useTexture('/brand-assets/backgrounds/noise-texture.jpg');

  return (
    <mesh>
      <sphereGeometry args={[1, 64, 64]} />
      <shaderMaterial
        uniforms={{
          uColor: { value: new THREE.Color(color) },
          uNoise: { value: noiseTexture },
          uTime: { value: 0 },
        }}
        // Custom vertex + fragment shaders for metallic radial sheen
      />
    </mesh>
  );
}

export function OrbScene() {
  return (
    <Canvas>
      <Orb color="#00d4aa" />
    </Canvas>
  );
}
```

The ElevenLabs orb uses:
- Perlin noise texture for surface detail
- Volume-reactive radius (expands with audio input)
- Color ramping based on state (listening, talking, thinking)
- Metallic radial sheen via fragment shader

## Layout Patterns

**Hero section accent:**
```html
<section class="hero-section" style="position: relative;">
  <img
    src="/brand-assets/voice-orbs/orb-teal-on-black.jpg"
    alt=""
    style="
      position: absolute;
      right: -5%;
      top: 50%;
      transform: translateY(-50%);
      width: 40%;
      mix-blend-mode: screen;
      opacity: 0.7;
    "
  />
  <!-- Hero content -->
</section>
```

**Card decoration:**
```html
<div class="card-darkflat" style="position: relative; overflow: hidden;">
  <img
    src="/brand-assets/voice-orbs/orb-purple-blue-on-black.jpg"
    alt=""
    style="
      position: absolute;
      right: -20%;
      bottom: -20%;
      width: 60%;
      mix-blend-mode: screen;
      opacity: 0.3;
    "
  />
  <div style="position: relative; z-index: 1;">
    <!-- Card content above the orb -->
  </div>
</div>
```
