# Bundled Assets

All assets live in `remotion/public/` and are referenced via `staticFile()`.

## Fonts (`public/fonts/`)

| File | Weight | Use |
|------|--------|-----|
| `KMR-Waldenburg-Buch.otf` | 300 (Book) | Large text — titles, headlines |
| `KMR-Waldenburg-Normal.otf` | 400 (Normal) | Medium text — card names, body |
| `KMR-Waldenburg-Halbfett.otf` | 600 (Semibold) | Small text — labels, tags |
| `KMR-Waldenburg-Fett.otf` | 700 (Bold) | Emphasis (rarely used) |

Loaded in `src/brand.ts`. Format must be specified as `"opentype"`.

## Logos (`public/logos/`)

| File | Use |
|------|-----|
| `Logo-black.png` | Full wordmark, light backgrounds (title cards) |
| `Logo-white.png` | Full wordmark, dark backgrounds (unused in current design) |
| `ElevenLabs_Icon_Black.svg` | II icon, light backgrounds (watermark, welcome card) |
| `ElevenLabs_Icon_White.svg` | II icon, dark backgrounds (unused) |

## Covers (`public/covers/`)

| File | Use |
|------|-----|
| `Cover0.jpg` | Chladni pattern photo — used for outro scenes, origin story |
| `Cover1.jpg` | Chladni pattern photo — used for title card (opener) |

Treatment: `grayscale(100%) contrast(1.1)`, opacity 0.25, gradient mask from left.

## Icons (`public/icons/`)

Product-specific icons for feature grids and card details:

| File | Represents |
|------|-----------|
| `tts.png` | Text to Speech |
| `dubbing.png` | Dubbing |
| `sfx.png` | Sound Effects |
| `voices.png` | Voice Library |
| `convai.png` | Conversational AI / Voice Agents |
| `stt.png` | Speech to Text / Speech Recognition |
| `home.png` | Home / Platform |

Not all scenes use image icons — many use inline SVGs for simpler iconography (question marks, checkmarks, etc.).

## Usage in Code

```tsx
import { staticFile, Img } from "remotion";

// Logo
<Img src={staticFile("logos/Logo-black.png")} style={{ height: 32 }} />

// Cover image
<Img src={staticFile("covers/Cover1.jpg")} style={{ width: "100%", objectFit: "cover" }} />

// Product icon
<Img src={staticFile("icons/tts.png")} style={{ width: 48, height: 48 }} />

// II watermark (handled by IIWatermark component)
<Img src={staticFile("logos/ElevenLabs_Icon_Black.svg")} style={{ height: 20 }} />
```
