# Project Scaffolding

How to create a new HyperFrames project from scratch for an ElevenLabs branded video — directory tree, default config, npm scripts, brand assets pre-wired.

## When to Scaffold

Scaffold automatically (without asking) when:

- The user names a project that doesn't exist yet (e.g., "build the H2 Marketing Demo in HyperFrames", "scaffold a new branded video for the launch event").
- A spec hand-off arrives for a project with no directory yet.

Skip scaffolding (proceed straight to scene building) when the project directory already exists with `index.html` and `meta.json`.

> **For Academy lessons specifically**, do NOT use this skill — switch to `academy-hyperframes`, which scaffolds inside the dedicated `~/Projects/hyperframes/` monorepo with the per-lesson directory + `shared/` symlink layout. This skill is for standalone branded videos.

## Composition ID vs Project Directory Name

The composition id (`meta.json#id`) is a versioned identifier the spec uses (e.g., `MarketingDemoH2v1`). The project directory name is usually the same id without the version suffix, or whatever the user already calls it. Both can match.

| Composition id (in `meta.json#id`) | Suggested directory |
|---|---|
| `MarketingDemoH2v1` | `marketing-demo-h2/` |
| `LaunchVideov2` | `launch-video/` |
| `InternalAllHandsv1` | `internal-allhands/` |

The voiceover sub-directory uses the full id: `<project>/assets/voiceover/<id>/`.

## Scaffolding Steps

Run from wherever the user wants the project to live (often `~/Projects/`). Substitute `<DIR>` (e.g. `marketing-demo-h2`) and `<ID>` (e.g. `MarketingDemoH2v1`) throughout.

### 1. Create directory tree

```bash
mkdir -p <DIR>/assets/{brand-assets,logos,fonts,voiceover/<ID>}
mkdir -p <DIR>/timing
mkdir -p <DIR>/renders
mkdir -p <DIR>/snapshots
mkdir -p <DIR>/scripts
cd <DIR>
```

### 2. Pull in brand assets

The `elevenlabs-brand-kit` plugin includes the canonical ElevenLabs brand assets at `<plugin-root>/brand-assets/`. Copy or symlink the parts you need into `<DIR>/assets/`:

```bash
# Backgrounds
cp -R <plugin-root>/brand-assets/{agents,chladni-closeup,general,gradient,creative} assets/brand-assets/backgrounds/

# Logos
cp <plugin-root>/brand-assets/logos/*.svg assets/logos/
cp <plugin-root>/brand-assets/logos/*.png assets/logos/ 2>/dev/null

# Icons (white variants are most often used on Hero scenes)
cp -R <plugin-root>/brand-assets/icons/white assets/brand-assets/icons/

# Fonts (KMR Waldenburg)
cp <plugin-root>/brand-assets/fonts/*.otf assets/fonts/
```

(In practice you may already have these in a sibling project — point the user to copy from there if so.)

### 3. Write `meta.json`

```json
{
  "id": "<ID>",
  "name": "<Project Title>",
  "createdAt": "<ISO-8601 now>"
}
```

### 4. Write `hyperframes.json`

```json
{
  "$schema": "https://hyperframes.heygen.com/schema/hyperframes.json",
  "registry": "https://raw.githubusercontent.com/heygen-com/hyperframes/main/registry",
  "paths": {
    "blocks": "compositions",
    "components": "compositions/components",
    "assets": "assets"
  }
}
```

### 5. Write `package.json`

```json
{
  "name": "<dir-name>",
  "version": "0.1.0",
  "private": true,
  "description": "<Project description>",
  "scripts": {
    "doctor":        "hyperframes doctor",
    "lint":          "hyperframes lint",
    "validate":      "hyperframes lint --verbose",
    "preview":       "hyperframes preview --port 3002",
    "snapshot":      "hyperframes snapshot",
    "timing:vo":     "node scripts/build-voiceover-timing.mjs",
    "transcribe:vo": "scripts/transcribe-voiceover.sh",
    "render:draft":  "npm run timing:vo && hyperframes render --quality draft --fps 30 --workers 1 --output renders/<ID>-draft.mp4 && cp timing/voiceover-captions.vtt renders/<ID>-draft.vtt",
    "render":        "npm run timing:vo && hyperframes render --quality standard --fps 30 --output renders/<ID>.mp4 && cp timing/voiceover-captions.vtt renders/<ID>.vtt"
  },
  "devDependencies": {
    "hyperframes": "0.4.20"
  }
}
```

Then `npm install`.

### 6. Drop in the brand surface

The `assets/academy-kit.css` and `assets/academy-kit.js` files are the universal ElevenLabs HyperFrames brand surface. Copy them from a known-good source:

- If you have an Academy project at `~/Projects/hyperframes/` — copy from `~/Projects/hyperframes/shared/academy-kit.{css,js}`.
- Otherwise copy from a previous standalone project, or generate fresh based on the patterns in `rules/brand-system.md` and `rules/animation-patterns.md`.

(The "academy-kit" naming is historical — these files apply to any ElevenLabs branded video, not just Academy lessons. You can rename them per-project but be sure to update `index.html` references too.)

### 7. Write `assets/voiceover/<ID>/manifest.json`

Empty chunk list to start. Populate it after VO generation.

```json
{
  "compositionId": "<ID>",
  "name": "<Project Title>",
  "version": "<Z>",
  "voice": { "id": "<ELEVENLABS_VOICE_ID>" },
  "model_id": "eleven_v3",
  "voice_settings": { "stability": 0.3, "similarity_boost": 0.75, "style": 0.6, "use_speaker_boost": true },
  "output_dir": "assets/voiceover/<ID>",
  "chunks": []
}
```

### 8. Write `index.html` skeleton

The skeleton is title-card + outro pre-wired with the academy-lockup pattern. The middle scenes get added during the spec build.

Reference any existing branded-video `index.html` for the working pattern — title-card lines (academy-lockup, hero-title with small/big/small wrap, accent-line, hero-subtitle, bottom-bar, full-bleed background) and outro (mirrors title-card with "Up Next" framing or similar). Key elements:

- `<link rel="stylesheet" href="assets/academy-kit.css" />`
- `<script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>`
- `<script src="assets/academy-kit.js"></script>`
- Root `<div id="root" data-composition-id="main" data-start="0" data-duration="<TOTAL>" data-width="1920" data-height="1080" data-fps="30">`
- Title-card `<section>` with hero-title (multi-line), academy-lockup at top, bottom-bar, full-bleed background
- Outro `<section>` with similar structure, closing copy
- Music bed `<audio id="music-bed">` at `data-volume="0.08"` covering full composition

Pre-fill the duration as a guess (~120s) — adjust after VO is timed.

### 9. Drop in the VO/timing scripts

Copy from a known-good source (sibling project's `scripts/` directory works):

- `scripts/transcribe-voiceover.sh` — Scribe v2 transcription of MP3s in `assets/voiceover/<ID>/`
- `scripts/build-voiceover-timing.mjs` — Joins index.html audio clips + Scribe JSON + manifest.json → `timing/voiceover-cues.md`, `timing/voiceover-word-timing.{json,csv}`, `timing/voiceover-captions.vtt`
- `scripts/generate-vo.mjs` — One-shot ElevenLabs `/with-timestamps` generator (writes MP3 + Scribe JSON in one API call)

Each script reads `meta.json#id` to find the project's voiceover dir, so they're portable across projects.

### 10. Verify the skeleton

```bash
npm run lint
```

Expect `0 errors` (the two pre-existing `duplicate_media_discovery_risk` warnings will appear once you add real watermarks; they're benign).

### 11. Print next-step guidance to the user

A short message like:

> Scaffolded `<DIR>/`. Skeleton has title-card + outro pre-wired with the academy-lockup pattern. Next: drop the spec at `<spec-path>`, or paste it inline. I'll read it (`rules/spec-reading.md`) and start building scenes per the spec's component list.

## After Scaffolding

The follow-up work is the real composition build:

1. **Read the spec** — see `rules/spec-reading.md`
2. **Map scenes** — see `rules/scene-patterns.md`
3. **Generate VO** — see `rules/composition-patterns.md` → "VO Generation"
4. **Wire animations** — see `rules/animation-patterns.md`
5. **Apply brand** — see `rules/brand-system.md`

The scaffolding only gets you to the starting line; everything else is building the video.
