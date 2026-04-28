---
name: hyperframes-builder
description: "Build HyperFrames video compositions from spec files for ElevenLabs branded videos. Takes structured spec markdown files (from the hyperframes-spec skill) and generates HyperFrames code: an index.html composition, GSAP timeline factory, scene-specific CSS, manifest.json, and meta.json. Use when: 'implement a spec', 'build a composition', 'code up the scenes', 'build this spec in hyperframes', 'add this scene', 'scaffold a new HyperFrames project', or any task involving writing HyperFrames composition code. Also trigger on 'make the video', 'build the HyperFrames for this', or when fixing/debugging HyperFrames rendering issues. Operates on the v2 ElevenLabs two-mode visual system (Hero/Content/Hybrid), KMR Waldenburg typography, brand terminology (platforms not pillars, ElevenLabs one word), deterministic GSAP motion, and the two-source-of-truth timing rule between index.html data-* attrs and the SCENES array in brand-kit.js. Use this skill for branded ElevenLabs videos such as product demos, marketing, internal comms, customer videos, and training content."
---

# ElevenLabs HyperFrames Builder

This skill takes a completed branded-video spec (from `hyperframes-spec` or written by hand) and builds the HyperFrames HTML composition that renders it. HyperFrames is HeyGen's HTML+GSAP video framework — `index.html` describes scenes via `<section>` elements with `data-start`/`data-duration`/`data-track-index`, and a paused GSAP timeline orchestrates entrances/exits.

For framework fundamentals — `data-*` semantics, `class="clip"` visibility, scene-transition rules, captions, audio handling — load the `hyperframes` skill (also in this plugin). For CLI commands (init, lint, preview, render, transcribe, tts), load `hyperframes-cli`.

## Project Structure Decision

Before scaffolding, decide which project structure fits:

| Type | Use For | Skill |
|---|---|---|
| **Shared monorepo project** (`~/Projects/hyperframes/<ProjectCode>/...`) | Teams that already keep videos in a shared HyperFrames root with `shared/` symlinks | Follow the local project-specific workflow for that monorepo |
| **Standalone project** | Product demos, marketing videos, internal comms, customer videos, training content, and other branded ElevenLabs videos | This skill |

If the spec belongs to an existing shared monorepo workflow, follow that workflow. Otherwise stay here and scaffold a standalone HyperFrames project.

## Your Role

You are the code agent. You receive a completed `{ProjectCode}-spec.md` file describing every scene in a video and produce inside the project root:

1. **`index.html`** — root composition with one `<section class="clip scene">` per spec scene, `data-start`/`data-duration`/`data-track-index` per scene, plus the VO `<audio>` clips and music bed at the bottom
2. **`assets/brand-kit.js`** — `SCENES` array matching `index.html` starts/durations, plus GSAP entrance tweens per scene (paused timeline registered on `window.createBrandTimeline`)
3. **`assets/brand-kit.css`** — any new scene-specific classes the spec requires (extend, don't inline)
4. **`meta.json`** — project id set to the composition id (e.g., `MarketingDemo2026v1`)
5. **`assets/voiceover/{composition-id}/manifest.json`** + VO MP3s — one per VO paragraph/beat

(The "brand-kit" naming is a historical artifact from this skill's origin in the branded video port. The CSS/JS files are the universal ElevenLabs HyperFrames brand surface — they apply to any branded video, not just course videos. You can keep the names or rename them per-project.)

## Design Principle — VO and Visuals Align Word-for-Word

Every branded video should have VO words lock to on-screen elements. Scenes must START when the VO introduces their subject. Card reveals must HIT on their keyword. Scene durations must MATCH VO phrasing. This is the single most important production value — the whole reason this stack uses HyperFrames over Remotion is to nail this alignment.

Apply this at every stage:

1. **When drafting a spec** (`hyperframes-spec` does this) — decide which VO phrase "belongs" to each scene. Scene.start should equal (VO phrase start). Scene.end should equal (VO phrase end + 1.5s crossfade tail).
2. **When generating VO** — know the scene list first. The VO must actually contain the words that map to visual elements (product names, card titles, caption phrases). Estimate pacing at ~145 WPM.
3. **After rendering VO** — run `npm run transcribe:vo` then `npm run timing:vo`. Use `timing/voiceover-word-timing.csv` as the source of truth for every scene timing decision. Don't estimate seconds by eye.
4. **When reviewing** — if the VO says a keyword and the visual for that keyword isn't on screen or actively arriving, the scene timing is wrong. Fix it.

Cue sheets are the contract between narration and visuals. See `rules/animation-patterns.md` → "Boom Pattern" for the within-scene word-sync recipe, and `rules/composition-patterns.md` → "Word-Cued Timing" for the cross-scene re-timing workflow.

## Before You Start — AlwaysThese

1. **Invoke `hyperframes`** — framework contract, `data-*` semantics, `class="clip"` visibility, scene-transition rules, audio handling, captions.
2. **Invoke `brand`** (also in this plugin) — brand terminology, typography foundation, color proportion rules.
3. **Invoke `gsap`** if you have it — GSAP timeline/tween patterns. Otherwise lean on `rules/animation-patterns.md`.
4. **Read the spec** — the v2 spec already specifies mode (Hero/Content/Hybrid), component, duration, and VO beat per scene. Treat it as the blueprint.
5. **Check whether the project dir exists** — if it doesn't, scaffold it first per `rules/project-scaffolding.md`, then proceed.

## Project Setup

Key facts for any HyperFrames project:

- **HyperFrames package:** `hyperframes` (dev dep — currently 0.4.20)
- **Resolution:** 1920×1080 (default for branded videos)
- **FPS:** 30
- **Transition overlap:** 1.5s (crossfade)
- **Studio preview port:** 3002 (default)
- **Renderer output:** `renders/{composition-id}.mp4` plus matching `.vtt` (auto-copied — see `rules/composition-patterns.md`)

### Standalone Project Layout

```
{project-name}/
├── index.html               # Root composition — every scene + audio clip
├── meta.json                # { "id": "<composition-id>", "name": "..." }
├── hyperframes.json         # Registry/paths config
├── package.json             # Scripts: preview, lint, render, timing:vo
├── assets/
│   ├── brand-kit.css      # Visual system — extend with new classes here
│   ├── brand-kit.js       # SCENES array + GSAP timeline factory
│   ├── brand-assets/        # Backgrounds, icons, fonts (from this plugin's brand-assets/)
│   ├── logos/               # ElevenLabs marks, product logos
│   ├── voiceover/<id>/      # 01-*.mp3 ... NN-*.mp3 + manifest.json
│   └── m1-1-background.mp3  # Music bed (or whatever filename you prefer)
├── timing/                  # Generated: scribe/, voiceover-cues.md, captions.vtt
├── renders/                 # Output MP4s + matching VTTs
├── snapshots/               # Generated frame stills
└── scripts/
    ├── transcribe-voiceover.sh   # Scribe v2 → timing/scribe/*.json
    ├── build-voiceover-timing.mjs # Joins clip starts + Scribe → timing/ files
    └── generate-vo.mjs            # ElevenLabs /with-timestamps generator (one-shot MP3 + Scribe JSON)
```

For full scaffolding steps (mkdir tree, default config files, npm scripts, asset paths), load `rules/project-scaffolding.md`.

## How to Use This Skill

### Step 1 — Read the Spec

The spec file tells you everything: mode per scene, component reference, layout, duration, VO beat. Load `rules/spec-reading.md` for how to map the spec's component names to HyperFrames scene patterns.

### Step 2 — Plan the Track Layout

HyperFrames stacks via `data-track-index` (integer, same-track clips cannot overlap). Adopt this convention:

- **Tracks 1–40**: scene `<section>` clips, one per scene, increment by 1 in scene order
- **Track 50–69**: VO audio clips (`vo-01`, `vo-02`, ...) — one per paragraph/beat
- **Track 72**: background music bed (single `<audio id="music-bed">`)

Set `z-index` on each `<section>` so adjacent scenes overlap correctly during the 1.5s crossfade (higher z-index on the earlier scene keeps it visible until its fade-out completes).

### Step 3 — Build `index.html`

Every scene becomes one `<section class="clip scene noise">` with:

```html
<section id="<scene-id>" class="clip scene noise"
         data-start="<seconds>" data-duration="<seconds>"
         data-track-index="<n>" style="z-index: <n>">
  <div class="scene-bg"><img src="assets/brand-assets/backgrounds/<path>.jpg" alt="" /></div>
  <!-- scene content: .hero-content / .scene-panel / .split-scene / etc. -->
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

VO clips go at the bottom:

```html
<audio id="vo-01" class="clip"
       data-start="0.5" data-duration="6.2" data-track-index="50"
       src="assets/voiceover/<comp-id>/01-opener.mp3"
       data-volume="1" preload="auto"></audio>
```

Music bed (single track, low volume bed under VO):

```html
<audio id="music-bed" class="clip"
       data-start="0" data-duration="<total>"
       data-track-index="72"
       src="assets/m1-1-background.mp3"
       data-volume="0.08" preload="auto"
       data-media-start="0"></audio>
```

**Generating music**: invoke the `/music` skill for an ElevenLabs Music API helper. A locked-in prompt that has worked well for narration-bed corporate videos:

> *"Upbeat, corporate explainer background music, inspirational with a modern, optimistic feel. Starting out right away, not a slow fade in. No vocals."*

Generate a track ~10s longer than the composition (`music_length_ms` in the API call), then drop it at `data-volume="0.08"` covering the full composition. No ducking layer needed at that volume.

Load `rules/scene-patterns.md` for the per-mode HTML templates (Hero/Content/Hybrid) and CSS class usage.

### Step 4 — Update `assets/brand-kit.js`

Keep `SCENES` in sync with the `<section>` starts/durations you wrote in `index.html`:

```js
const SCENES = [
  { id: "title-card", start: 0, duration: 10 },
  { id: "text-reveal", start: 8.5, duration: 14.48 },
  // ...
];
const TOTAL_DURATION = <sum minus overlaps>;
```

Build entrance animations inside `createBrandTimeline()` using `fromToIf()`/`toIf()` helpers (see reference implementation). Load `rules/animation-patterns.md` for GSAP timing patterns.

### Step 5 — Wire VO

Drop the VO MP3s into `assets/voiceover/<comp-id>/` with a `manifest.json`. Add one `<audio id="vo-NN" class="clip">` per chunk in `index.html`. Then run:

```bash
npm run transcribe:vo   # only if MP3s came from outside TTS — needs ELEVENLABS_API_KEY
npm run timing:vo       # always — rebuilds timing/voiceover-cues.md and captions.vtt
```

**For freshly generated VO** (the preferred path): use `scripts/generate-vo.mjs`, which calls the ElevenLabs `/text-to-speech/{voice_id}/with-timestamps` endpoint and writes both the MP3 AND the Scribe v2-format JSON in one shot — no `transcribe:vo` round-trip needed. Use the project-specified voice with `eleven_v3` and locked generation settings (stability 0.3, style 0.6, similarity 0.75, speaker boost on) so generated clips match the approved production narration for that project. Full recipe + the regeneration/splitting workflow lives in `rules/composition-patterns.md` → "VO Generation".

The `/text-to-speech` skill is the general-purpose ElevenLabs TTS reference — invoke it when picking a voice, exploring `eleven_v3` audio tags (`[warm]`, `[confident]`, `[chuckle]`, `[pause]`, `[excited]`), or generating a one-off line outside the composition's manifest. The `generate-vo.mjs` script is the project-specific wrapper that integrates TTS output into the timing pipeline.

### Step 6 — Verify

```bash
npm run lint            # must pass before presenting work
npm run validate        # verbose — surfaces info-level findings
npx hyperframes snapshot --at 2
npx hyperframes snapshot --at <mid-scene-ts>
npm run preview         # Studio on :3002 for full playback review
npm run render:draft    # only if timing/audio/export behavior needs a real MP4 — auto-copies VTT
```

Fix all lint errors. Spot-check with snapshots before paying for a full render.

## Core Rules — Non-Negotiable

Load `rules/` files for detailed patterns. Essentials:

### Framework contract (HyperFrames)

1. Every timed element needs `data-start`, `data-duration`, `data-track-index`
2. Visible timed elements MUST have `class="clip"` — the runtime uses it for visibility
3. GSAP timelines must be paused; expose via `window.createBrandTimeline` (the HyperFrames runtime registers it on `window.__timelines`)
4. `<video>` uses `muted`; audio goes on a separate `<audio>` clip
5. Deterministic only — no `Date.now()`, `Math.random()`, network fetches
6. Animate opacity + transforms only — NEVER `display` or `visibility`
7. No `repeat: -1` — calculate finite repeats from composition duration
8. Build timelines synchronously (no `async`/`setTimeout`/`Promise`)

### Two-source-of-truth — keep in sync

- `<section>` `data-start`/`data-duration` in `index.html` drive the runtime
- `SCENES` array in `assets/brand-kit.js` drives the GSAP timeline
- Hard-coded absolute cue times inside `fromToIf`/`toIf` (e.g., `22.88`, `85.63`) drive specific entrance timing

Change any one → update the others. Run `npm run timing:vo` after any audio-start change.

### Brand system (v2 two-mode)

Load `rules/brand-system.md` for the complete reference. Summary:

- **Hero mode** — full-bleed `<div class="scene-bg">` background image + white text + `.dark-card` translucent cards. Watermark white at 25% opacity. Use for title, origin, methodology, outros.
- **Content mode** — OFF_WHITE (`#FAFAFA`) scene background + GRAPHITE (`#1E1916`) text + solid CREAM (`#F5F3F1`) cards with LIGHT_GRAY borders. Watermark GRAPHITE at 35%. Use for step sequences, detail content, checklists.
- **Hybrid mode** — white left 45% (GRAPHITE text) + gradient right 55% (frosted cards). Watermark GRAPHITE. Use for module maps, timelines.

### Animation system

Load `rules/animation-patterns.md` for the full conversion table. Essentials:

| Remotion (@30fps) | HyperFrames (GSAP) |
|-------------------|--------------------|
| `spring({ damping: 200 })` | ease `"power2.out"` / `"power3.out"` — never bouncy |
| `TRANSITION_FRAMES = 45` | 1.5s crossfade (controlled by scene fade in+out in `fadeScene`) |
| `ENTER_DELAY = 35` | entrance offset `scene.start + 0.38s` from scene start (account for crossfade overlap) |
| Stagger 8–20f | `0.08–0.2s` |
| Exit fade `[-40, -10]` | `tl.to(sel, { opacity: 0, duration: 0.75 }, scene.start + scene.duration - 0.85)` |

Rules:
- Scene transitions: fade only via `fadeScene(tl, scene)` helper
- Apply exit to scene `<section>` — do NOT animate `.scene-bg` separately during exit
- Vary easings across a scene (≥3 different eases for choreography variety)
- Offset the first entrance 0.1–0.3s after scene start (not t=0) — content appears after crossfade settles

### Brand terminology (always enforced)

- "ElevenLabs" — one word, capital E, capital L — never "Eleven Labs", "Elevenlabs", "ELEVENLABS"
- Three **platforms** (never "pillars"): ElevenCreative, ElevenAgents, ElevenAPI
- "ElevenLabs" for the subtitle on title cards when relevant
- The II symbol is visual only — never typed as text

For complete brand terminology, invoke the `brand` skill in this plugin.

## Rules Directory

Load the file that matches what you're building:

| File | When to load |
|------|--------------|
| `rules/project-scaffolding.md` | Creating a new HyperFrames project from scratch — directory tree, default config files, npm scripts |
| `rules/brand-system.md` | Creating any visual element — colors, typography, card patterns, watermark rules, mode switch |
| `rules/scene-patterns.md` | Writing a new scene — HTML templates for Hero/Content/Hybrid, `.split-scene`, card grids, product banners |
| `rules/animation-patterns.md` | Adding GSAP tweens — Remotion frames → GSAP seconds, entrance patterns, crossfade rules |
| `rules/composition-patterns.md` | Building `index.html` end-to-end — track layout, z-index stacking, audio track, `createBrandTimeline` structure, `meta.json`, `hyperframes.json` |
| `rules/spec-reading.md` | Parsing a HyperFrames-spec output (or legacy Remotion spec) — mapping component names to scene patterns |

## References Directory

| File | Purpose |
|------|---------|
| `references/existing-classes.md` | Full inventory of `assets/brand-kit.css` classes with use notes |
| `references/bundled-assets.md` | Brand asset library map (backgrounds, voice-orbs, icons, logos) |

## Related Skills

These skills aren't HyperFrames-specific but are essential to the audio side of any branded video build:

| Skill | When to invoke |
|---|---|
| `text-to-speech` | Generating a VO line, picking a voice, or referencing the `eleven_v3` audio-tag vocabulary. The project-specific `generate-vo.mjs` wraps this for batch use; for one-offs or custom calls, invoke `/text-to-speech` directly. |
| `music` | Generating a music bed via the ElevenLabs Music API. Use the corporate-explainer prompt for narration-bed videos; aim for `music_length_ms` ~10s longer than the composition so the bed always covers the outro tail. |
| `speech-to-text` | Transcribing existing MP3s when you don't have the source text (Scribe v2). The project's `transcribe-voiceover.sh` wraps this; the `/speech-to-text` skill is the general reference. |
| `brand` | Brand terminology + brand-asset usage rules. Always relevant when authoring scene copy, icon choices, or color decisions. |
| `hyperframes` | Official HeyGen HyperFrames runtime reference — load when you need data-attr semantics, audio handling, captions, transitions, or audio-reactive patterns beyond what's in this skill. |
| `hyperframes-cli` | Official HeyGen CLI reference — load for invocation help on init/lint/preview/render/transcribe/tts/doctor/etc. |

## Common Mistakes to Avoid

1. **Forgetting `class="clip"`** on a timed element — framework hides it entirely. Every `<section>` and every `<audio>`/`<video>` needs it.
2. **Desyncing `SCENES` and `index.html` `data-start`** — visual content will no longer line up with scene boundaries.
3. **Using `display`/`visibility` in GSAP** — breaks the capture engine. Only animate opacity + transforms.
4. **Building the timeline async** — runtime reads `window.__timelines` synchronously after page load. Build inline.
5. **Skipping the scene entrance offset** — content appears during crossfade and looks jumpy. Offset first entrance 0.3–0.4s after `scene.start`.
6. **Forgetting `.watermark`** on non-title scenes — the persistent II mark must be on every scene. Use `.watermark.white` on Hero, default on Content.
7. **Using black/cream icons on Hero** — Hero icons are always white. Use `assets/brand-assets/icons/white/` variants, or apply `filter: brightness(0) invert(1)` to product icons.
8. **Using `repeat: -1`** — breaks capture. Always calculate finite repeat counts from duration.
9. **Hardcoding a pixel viewport width** in CSS — scenes must fill via percentages/padding so Studio resizing works.
10. **`<br>` in wrapping body text** — produces unexpected breaks at rendered font size. Use `max-width` on the container and let text wrap naturally. Exception: short display titles where each word is deliberately on its own line.
11. **Inline scene-specific styles in `index.html`** — extend `assets/brand-kit.css` instead. Inline style blocks bloat the composition and lose reusability.
12. **Dimming the II icon on Hero** — title/welcome/outro watermarks are full opacity. Never apply `opacity: 0.4` to the logo mark.
13. **Ignoring an existing monorepo workflow** — for videos already managed inside a shared HyperFrames monorepo, follow that repo's local scaffolding rules. Use this skill for standalone branded videos.

## Output Checklist

Before presenting work to the user:

- [ ] `npm run lint` passes (errors = 0)
- [ ] `SCENES` array matches every `<section>`'s `data-start`/`data-duration` in `index.html`
- [ ] Every `<section class="scene">` carries `class="clip"`, `data-*` attrs, and a `z-index` that respects the 1.5s crossfade overlap
- [ ] Every scene has a `.watermark` (white on Hero, default on Content) except the title card's deliberate exception if any
- [ ] VO `<audio>` clips are all on their own track indexes (50–69), music bed on 72
- [ ] `assets/voiceover/<comp-id>/manifest.json` exists and lists the MP3s
- [ ] `npm run timing:vo` has been re-run since the last audio-start change
- [ ] Snapshots at representative scenes (title + mid + outro) look on-brand
- [ ] No CSS `display`/`visibility` animation, no `Math.random()`, no `Date.now()`, no `repeat: -1`

## Project Context

This skill replaces `remotion-builder` (deprecated). The HyperFrames runtime gives faster iteration (Studio playback, hot reload, snapshot speed) and easier audio editing than the Remotion render loop. New branded videos go through `hyperframes-spec` to produce the spec, then this skill to build the composition.

If you're building inside an existing shared HyperFrames monorepo, follow that repo's local scaffolding workflow. This skill is for standalone branded videos.
