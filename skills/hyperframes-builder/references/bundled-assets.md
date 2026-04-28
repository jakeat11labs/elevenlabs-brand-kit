# Bundled Assets Inventory

This brand asset library mirrors the Remotion project's. Everything lives under `assets/` at the project root.

## Reference Project

The canonical bundled assets live in `~/Projects/hyperframes/assets/` (the M01L01v6 viability port). When starting a new lesson, copy this tree into the new project; then add any lesson-specific assets alongside.

## Brand Assets (Mirrored from Remotion)

`assets/brand-assets/` — full brand asset library, parallel to Remotion's `public/brand-assets/`.

### Backgrounds

`assets/brand-assets/backgrounds/`

| Category | Count | Location | Feel / Use |
|----------|-------|----------|------------|
| Agents | 9 | `backgrounds/agents/agents-bg-NN-*.jpg` | Green, teal, blue-warm. Agent-related Hero scenes. |
| Chladni closeup | 15 | `backgrounds/chladni-closeup/closeup-NN-*.jpg` | Diverse natural, mixed warm/cool. Concept reveals, emotional moments. |
| General | 7 | `backgrounds/general/general-bg-NN-*.jpg` | Blue/cyan with Chladni overlay. Title cards, generic Hero. |
| Gradient | 16 | `backgrounds/gradient/Background0.jpg` ... `Background15.jpg` + `Background9bw.jpg` + `Background16.jpg` | Atmospheric gradients. Clean, versatile default. |
| Creative | 1 | `backgrounds/creative/*.jpg` | Coral sunset. Creative/warm accent. |
| Shared textures | 2 | `backgrounds/noise-texture.jpg`, `backgrounds/seamless-texture.jpg` | For overlays and tiled backgrounds |

Background selection tips:

- Pick by tonal fit, not literal subject match. The compositing pipeline (image → blur → tint → noise → content) normalizes all Hero backgrounds.
- `backgrounds/gradient/` is the safest default for any Hero scene.
- `backgrounds/agents/` for any scene about ElevenAgents specifically.
- `backgrounds/chladni-closeup/` for research/voice/origin narrative moments.
- Don't use `backgrounds/creative/` except on creative-specific Hero scenes — the warm coral clashes with most narratives.

### Icons

`shared/brand-assets/icons/`

| Category | Count | Location | Use |
|----------|-------|----------|-----|
| White | ~130 | `icons/white/NNN-name.png` | Hero scenes (white on dark/gradient backgrounds) |
| Black | ~130 | `icons/black/NNN-name.png` | Content scenes (black on off-white) |
| Cream | ~130 | `icons/cream/NNN-name.png` | Special warm treatments — rarely used in branded ElevenLabs context |
| Product | ~10 | `icons/product/{tts,stt,convai,dubbing,studio,...}.png` | Product icons — always color, apply `filter: brightness(0) invert(1)` for white variant on Hero |

Icon naming: `NNN-descriptor.png` where NNN is a stable catalog number. Keep the number in filenames when copying — makes cross-referencing with the Remotion asset catalog trivial.

### Voice Orbs

`assets/brand-assets/voice-orbs/` — 8 metallic orbs on black backgrounds.

Colors available: teal, purple-blue, pink, coral, green, gold, hotpink, lavender.

Composite with `mix-blend-mode: screen` over any background — the black substrate drops out and the orb glow layers on top.NOT use without `mix-blend-mode: screen` or the orbs will have visible black boxes.

### Diagrams

`assets/brand-assets/diagrams/` — platform architecture diagram.

Use sparingly — this is a specific ElevenAgents architecture illustration. When a scene needs a bespoke diagram, draw it with SVG in the scene markup or add a new asset.

### UI Highlights

`assets/brand-assets/ui-highlights/` — phone agent live activity screenshot.

Used in phone deployment / integration scenes. One asset currently; extend if new UI captures are needed.

### Brand Meta

| File | Purpose |
|------|---------|
| `assets/brand-assets/color-tokens.json` | Token definitions — mirror of Remotion `public/brand-assets/color-tokens.json` |
| `assets/brand-assets/BRAND_GUIDELINES.md` | Official 2025 brand rules |
| `assets/brand-assets/CLAUDE.md` | Agent-readable asset guide (same one referenced by `elevenlabs-brand` skill) |

## Project-Local Assets

### Logos

`assets/logos/`

| File | Purpose |
|------|---------|
| `ElevenLabs_Icon_White.svg` | White II icon — Hero watermarks |
| `ElevenLabs_Icon_Black.svg` | Black II icon — Content watermarks |
| `elevenlabs-icon-white.svg` | Brand lockup icon (white) |
| Other logos as needed | Module-specific or lesson-specific variants |

### Fonts

`assets/fonts/` — KMR Waldenburg OTF files, embedded automatically by the HyperFrames compiler.

| File | Weight |
|------|--------|
| `KMR-Waldenburg-Buch.otf` | 300 (Light / Book) |
| `KMR-Waldenburg-Normal.otf` | 400 (Regular) |
| `KMR-Waldenburg-Halbfett.otf` | 600 (Semibold) |

Never use 700 (Fett) for headlines. It ships with the family but isn't part of the official hierarchy.

### Top-level loose icons

`assets/icons/` — a flat collection of loose PNGs used directly in compositions without the `brand-assets/` nesting. These are essentially project-local convenience copies of the most-used icons:

`book.png`, `brain.png`, `checkmark.png`, `convai.png`, `dubbing.png`, `explosion.png`, `group.png`, `home.png`, `music.png`, `phone.png`, `play.png`, `sfx.png`, `stt.png`, `studio.png`, `tts.png`, `video.png`, `voice-changer.png`, `voice-isolator.png`, `voices.png`, `window-app.png`, `zap.png`.

Prefer `brand-assets/icons/white/` or `brand-assets/icons/black/` over these loose copies when you want a guaranteed white/black variant. The loose icons are whatever color the PNG ships with.

### Generated Imagery

`assets/generated/` — certification artwork and any other imagery produced specifically for this video. Typically not reused across lessons.

### Specific Assets

| File | Use |
|------|-----|
| `assets/shield.png` / `assets/shield-trimmed.png` | Certification shield imagery (cert scene) |
| `assets/m1-1-background.mp3` | Background music bed (M01L01 only; each lesson picks its own) |

## Voiceover

`assets/voiceover/{composition-id}/` — one folder per lesson.

- `{NN}-{slug}.mp3` — chunked VO, one per paragraph/beat (e.g., `01-voice-opener.mp3`, `02-research-origin.mp3`, ...)
- `manifest.json` — manifest consumed by `scripts/build-voiceover-timing.mjs`:

```json
{
  "compositionId": "M01L01v6",
  "lesson": "M01L01",
  "sourceScript": "[full VO script as one block]",
  "chunks": [
    {
      "id": "01-voice-opener",
      "scenes": ["title-card"],
      "text": "[paragraph text]"
    }
    // ...
  ]
}
```

The `id` matches the MP3 filename without extension. The `scenes` array lists which scene ids this VO clip spans. The `text` is the VO paragraph text.

## Asset Pattern When Porting a New Lesson

1. Copy `~/Projects/hyperframes/assets/brand-assets/` into the new project (don't symlink — HyperFrames needs the files colocated)
2. Copy `assets/fonts/`, `assets/logos/`, `assets/icons/` verbatim
3. Create `assets/voiceover/<new-comp-id>/` with the new lesson's MP3s + manifest
4. Pick an appropriate background music bed (may be different per lesson) — drop at `assets/<music>.mp3`
5. Skip `assets/generated/` unless the new lesson has bespoke imagery
6. `assets/shield.png` only if the lesson is the cert closing lesson

## Asset Rules

1. **Never reference assets outside `assets/`** from `index.html` or `brand-kit.css` — HyperFrames won't bundle external paths.
2. **Use relative paths** (`assets/brand-assets/...`) not absolute (`/assets/...` or `file:///...`).
3. **Colocate fonts, images, and audio** — the compiler bundles from `assets/` only.
4. **White icons on Hero, black/graphite on Content.** If the exact icon doesn't exist in the needed color, apply CSS filter:
   ```css
   .feature-icon.invert { filter: brightness(0) invert(1); }  /* any → white */
   .feature-icon.darken { filter: brightness(0); }            /* any → black */
   ```
5. **Never commit secrets** — API keys go through environment variables (`ELEVENLABS_API_KEY` for Scribe transcription). The shell script `scripts/transcribe-voiceover.sh` reads it from the environment; no keys in source.
