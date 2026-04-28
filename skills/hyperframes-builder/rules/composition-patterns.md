# Composition Patterns for HyperFrames

How a lesson's composition fits together end-to-end: tracks, z-stacking, audio, manifest, meta, scripts.

## Track Layout

HyperFrames `data-track-index` is a scheduling dimension — same-track clips cannot overlap in time. It does NOT affect visual layering (use CSS `z-index` for that).

This project's conventions:

| Track range | Purpose |
|-------------|---------|
| **1–40** | Scene `<section>` clips — one per scene, incrementing in scene order |
| **50–69** | VO audio clips (`vo-01`, `vo-02`, ...) — one per paragraph/beat |
| **72** | Background music bed — single `<audio id="music-bed">` |

Scene tracks can re-use indexes if you split the composition into phases that never overlap, but default to unique indexes per scene for clarity. VO tracks can occasionally overlap if two voices/clips must play simultaneously — put each on its own index.

## Z-Index Stacking

During a 1.5s crossfade, two scenes are in the DOM simultaneously. The outgoing scene must sit on top of the incoming one until its fade-out completes.

Use monotonically decreasing `z-index` across scenes in `index.html`:

```html
<section id="title-card"  style="z-index: 99">...</section>
<section id="text-reveal" style="z-index: 98">...</section>
<section id="origin"      style="z-index: 97">...</section>
<!-- ... -->
<section id="outro"       style="z-index: 80">...</section>
```

`.scene-bg`, `.watermark`, and decoratives inside each scene use their own local z-index values inside the scene's stacking context — they don't need to coordinate with neighboring scenes.

## Audio Track

### Voiceover clips

Each VO paragraph/beat is one `<audio>` clip:

```html
<audio id="vo-01" class="clip"
       data-start="0.5" data-duration="6.2" data-track-index="50"
       src="assets/voiceover/M02L03v1/01-opener.mp3"
       data-volume="1" preload="auto"></audio>
<audio id="vo-02" class="clip"
       data-start="8.3" data-duration="5.1" data-track-index="51"
       src="assets/voiceover/M02L03v1/02-research.mp3"
       data-volume="1" preload="auto"></audio>
<!-- ... vo-03 through vo-NN ... -->
```

Put each VO on its own track index (50, 51, 52, ...). `data-duration` should match the MP3 length exactly — Scribe reports `audio_duration_secs` in `timing/scribe/<id>.json` after `npm run transcribe:vo:<lc>`.

`data-start` is the absolute composition time when the VO begins playing. Studio edits to VO starts update this attribute; run `npm run timing:vo:<lc>` afterwards to rebuild the cue sheet.

### Music bed

A single `<audio id="music-bed">` on track 72 at `data-volume="0.08"`:

```html
<audio id="music-bed" class="clip"
       data-start="0" data-duration="<total>"
       data-track-index="72"
       src="assets/m1-1-background.mp3"
       data-volume="0.08" preload="auto"
       data-media-start="0"></audio>
```

> **Related skill**: `/music` is the general-purpose ElevenLabs Music API reference. Generate the bed track with a corporate-explainer prompt — a locked-in version that has worked well for narration-bed videos:
>
> *"Upbeat, corporate explainer background music, inspirational with a modern, optimistic feel. Starting out right away, not a slow fade in. No vocals."*
>
> Generate ~10s longer than the composition (`music_length_ms` in the API call) so the bed always covers the outro tail. At `data-volume="0.08"`, no manual ducking layer is needed — VO sits cleanly above the bed.

If the music file is shorter than the composition, either:
- **Extend**: swap in a longer source file (regenerate via `/music` skill at a longer length)
- **Loop**: split into adjacent `<audio>` clips that together cover the composition
- **Trim-and-end-early**: accept that the bed ends before the final outro beat

For **manual ducking** (rarely needed at `data-volume="0.08"`): split the music bed into adjacent clips with different `data-volume` values around dense VO sections.

## VO Manifest

`assets/voiceover/{composition-id}/manifest.json` describes the VO chunks and their script beats. Shape:

```json
{
  "compositionId": "M02L03v1",
  "lesson": "M02L03",
  "sourceScript": "... full VO script text ...",
  "chunks": [
    {
      "id": "01-opener",
      "scenes": ["title-card"],
      "text": "Welcome back to ElevenLabs Academy. Today we're going to..."
    },
    {
      "id": "02-research",
      "scenes": ["origin", "evolution"],
      "text": "ElevenLabs was founded as a research company..."
    }
    // ...
  ]
}
```

- `chunks[i].id` matches `assets/voiceover/{comp-id}/{id}.mp3` exactly (without `.mp3`)
- `chunks[i].scenes` lists which scene ids this VO clip spans — useful for cross-reference
- `chunks[i].text` is the VO script paragraph, used by `scripts/build-voiceover-timing.mjs`

## Scripts

Two Node/shell scripts live at `scripts/` (used by every lesson; they read `meta.json#id` to find the lesson's voiceover dir):

### `scripts/transcribe-voiceover.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

if [[ -z "${ELEVENLABS_API_KEY:-}" ]]; then
  echo "ELEVENLABS_API_KEY is required" >&2
  exit 1
fi

audio_dir="assets/voiceover/<comp-id>"
out_dir="timing/scribe"
mkdir -p "$out_dir"

for audio in "$audio_dir"/*.mp3; do
  id="$(basename "$audio" .mp3)"
  curl -fsS -X POST "https://api.elevenlabs.io/v1/speech-to-text" \
    -H "xi-api-key: $ELEVENLABS_API_KEY" \
    -F "file=@$audio" \
    -F "model_id=scribe_v2" \
    -F "timestamps_granularity=word" \
    -F "language_code=eng" \
    -F "keyterms=ElevenLabs" -F "keyterms=ElevenAgents" \
    -F "keyterms=ElevenCreative" -F "keyterms=ElevenAPI" \
    -F "keyterms=REST API" -F "keyterms=SDKs" -F "keyterms=CLI" \
    -F "keyterms=SOC 2" -F "keyterms=HIPAA" -F "keyterms=PCI DSS" \
    -F "keyterms=ISO 27001" -F "keyterms=GDPR" -F "keyterms=WhatsApp" \
    -o "$out_dir/$id.json"
done
```

Update the `keyterms` list to match any new proper nouns introduced by the video's VO — it gives Scribe hints for accurate transcription.

### `scripts/build-voiceover-timing.mjs`

This script reads every `<audio id="vo-*">` from `index.html`, joins it with `manifest.json` and the Scribe JSON files, and writes:

- `timing/voiceover-cues.md` — readable absolute-time cue sheet
- `timing/voiceover-word-timing.json` — structured clips/cues/words
- `timing/voiceover-word-timing.csv` — flat word-level sheet

The reference implementation lives at `~/Projects/hyperframes/scripts/build-voiceover-timing.mjs` and is shared across every lesson — no per-lesson copying needed. It reads `meta.json#id` to find the lesson's voiceover dir.

### `package.json` scripts

```json
{
  "scripts": {
    "doctor": "hyperframes doctor",
    "lint": "hyperframes lint",
    "validate": "hyperframes lint --verbose",
    "preview": "hyperframes preview --port 3002",
    "render:draft": "hyperframes render --quality draft --fps 30 --workers 1 --output renders/<comp-id>-hyperframes-draft.mp4",
    "render": "hyperframes render --quality standard --fps 30 --output renders/<comp-id>-hyperframes.mp4",
    "snapshot": "hyperframes snapshot",
    "transcribe:vo": "scripts/transcribe-voiceover.sh",
    "timing:vo": "node scripts/build-voiceover-timing.mjs"
  }
}
```

## `meta.json`

```json
{
  "id": "<comp-id>",
  "name": "<comp-id>",
  "createdAt": "<ISO-8601>"
}
```

`id` matches the composition id (e.g., `M02L03v1`) used in file names, manifest, and render outputs.

## `hyperframes.json`

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

Paths section tells `hyperframes add` where to install blocks/components. Don't change these unless the project structure differs from the reference.

## VO Generation (one-shot: audio + timing in one API call)

When generating new VO clips for a branded video, use the ElevenLabs `/text-to-speech/{voice_id}/with-timestamps` endpoint instead of generate-then-transcribe. It returns audio + character-level alignment in a single request — no separate Scribe API call needed, which saves cost AND keeps generation deterministic with the timing data.

The canonical generator lives at `scripts/generate-vo.mjs`. Copy this pattern into every new HyperFrames project.

> **Related skill**: `/text-to-speech` is the general-purpose ElevenLabs TTS reference — invoke it when you need to choose a voice, look up the `eleven_v3` audio-tag vocabulary (`[warm]`, `[confident]`, `[chuckle]`, `[pause]`, `[excited]`, etc.), or generate a one-off line outside the composition's manifest. The `generate-vo.mjs` script wraps `/text-to-speech` for batch project use, locking voice settings + integrating with the timing pipeline. For ad-hoc voice work or experimenting with new voices, go straight to the skill.

### Locked generation settings

These are the production-aligned model/settings. Pair them with the voice approved for the current project; do not hard-code a default voice in shared skills. DO NOT tweak these settings without explicit approval, or new clips will sound different from existing ones.

```js
const VOICE = process.env.ELEVENLABS_VOICE_ID;
if (!VOICE) {
  throw new Error("Set ELEVENLABS_VOICE_ID for this project.");
}
const MODEL = "eleven_v3";
const SETTINGS = {
  stability: 0.3,
  similarity_boost: 0.75,
  style: 0.6,
  use_speaker_boost: true,
};
```

These match the `generate-narration.ts` settings in the sibling ElevenLabs project while leaving voice selection project-specific. **Note**: these differ from the older settings (`stability: 0.5, style: 0.3`) seen in some manifest.json files — the locked production settings above take precedence.

### API endpoint

```
POST https://api.elevenlabs.io/v1/text-to-speech/{voice_id}/with-timestamps?output_format=mp3_44100_64
```

Body:
```json
{
  "text": "Your VO text here. Inline tags like [confident] or [pause] supported.",
  "model_id": "eleven_v3",
  "voice_settings": { ... }
}
```

Response shape:
```ts
{
  audio_base64: string;            // decode → write .mp3
  alignment: {
    characters: string[];
    character_start_times_seconds: number[];
    character_end_times_seconds: number[];
  };
  normalized_alignment?: { ... };  // prefer this — handles audio tags better
}
```

### Output layout (mirrors existing pipeline)

For each clip:
- **MP3:** `assets/voiceover/{comp-id}/{clip-id}.mp3`
- **Scribe v2-shaped JSON:** `timing/scribe/{clip-id}.json` — generated from the `/with-timestamps` alignment, NOT from a Scribe API call. The existing `scripts/build-voiceover-timing.mjs` reads this file unchanged.

The Scribe-format JSON schema:
```json
{
  "audio_duration_secs": <last char end time>,
  "language_code": "eng",
  "language_probability": 1.0,
  "text": "<original text with [tags] stripped>",
  "words": [
    { "type": "word", "start": 0.099, "end": 0.459, "text": "Before" },
    { "type": "spacing", "start": 0.459, "end": 0.499, "text": " " },
    { "type": "word", "start": 0.499, "end": 1.099, "text": "screens," },
    ...
  ]
}
```

### Character → word conversion

The `/with-timestamps` response is character-level. Convert to Scribe word-level by:

1. Walk characters, tracking timestamps.
2. Skip `[tag]` sequences entirely (audio tags shouldn't appear in transcripts; the silence they produce naturally becomes a gap in word timestamps).
3. Group consecutive non-whitespace into a single `"word"` entry (trailing punctuation rides with the word — matches Scribe v2 behavior, e.g. `"screens,"` is one word).
4. Group consecutive whitespace into `"spacing"` entries.
5. The word's `start` is the first character's `start_time`; `end` is the last character's `end_time`.

Reference implementation: `scripts/generate-vo.mjs` → `charToScribeWords()`.

### Workflow for regenerating / splitting / adding VO

1. **Edit `CLIPS` array in `scripts/generate-vo.mjs`** — list each clip with `{id, text}`. IDs follow the project naming (`NN-slug` or `NN.MM-slug` for sub-clips like `06.01-three-things`).

2. **Source the API key from a sibling ElevenLabs project** — never copy keys into the HyperFrames repo:
   ```bash
   set -a; source <sibling-project>/.env.local; set +a
   node scripts/generate-vo.mjs
   ```

3. **Update `assets/voiceover/{comp-id}/manifest.json`** — replace/insert chunk entries to match new clip IDs. Keep chunks in compositional order.

4. **Update `index.html`** — add/replace `<audio>` element(s) with new clip IDs, durations (from script output), and track indexes. Each new clip gets its own track index in the 50–69 (or project-specific) VO range.

5. **Run `npm run timing:vo:<lc>`** — rebuilds `timing/voiceover-cues.md` + `voiceover-word-timing.csv` from the current `<audio data-start>` values + the new Scribe JSONs. **Do NOT run `transcribe:vo`** — the Scribe JSON was already written by `generate-vo.mjs`.

6. **Archive old MP3s + JSONs** — move into `assets/voiceover/{comp-id}/_archive/` and `timing/scribe/_archive_*.json` rather than deleting, so you can roll back if a regeneration sounds wrong.

7. **Lint + snapshot** to verify.

### When to use which generation path

| Situation | Use |
|-----------|-----|
| New VO clip from scratch | `scripts/generate-vo.mjs` (with-timestamps) |
| Splitting an existing clip into multiple | `scripts/generate-vo.mjs` (regenerate as separate clips) |
| Re-recording an existing clip with edited text | `scripts/generate-vo.mjs` |
| MP3 already exists, only need timing data | `scripts/transcribe-voiceover.sh` (Scribe v2) — fallback when no source text or for legacy files |
| Audio file was edited externally (not via TTS) | `scripts/transcribe-voiceover.sh` |

The `transcribe-voiceover.sh` path remains the right tool when you don't have the source text or the MP3 was generated/edited outside the TTS flow. For any clip you're generating fresh, `generate-vo.mjs` is the way.

### Example: splitting one clip into two

Original chunk in manifest: `06-three-things-tease` — "So what makes ElevenAgents different…? [pause] Three things."

After split:
- `06-makes-different` — "So what makes ElevenAgents different from every other platform out there?"
- `06.01-three-things` — "[confident] Three things."

Each becomes its own draggable clip in Studio. The `06.01` numbering preserves manifest ordering without renumbering subsequent clips.

## Audio-Repositioning Workflow (Studio-first, code-follows)

The user can drag `<audio>` clips directly in Hyperframes Studio — Studio edits `data-start` in `index.html`. Visual scenes are code-driven. The division of labor:

- **The user owns audio placement** (via Studio drag)
- **Claude owns visual scene timing** (via edits to `index.html` + `SCENES` array)

**When"I moved [audio]" / "I repositioned things" / "the [word] now plays at [time]":**

1. **Run `npm run timing:vo:<lc>` FIRST.** This re-reads current `<audio data-start>` values and regenerates `timing/voiceover-cues.md` + `voiceover-word-timing.csv` with new absolute composition times. Without this, you're looking up stale word times.

2. **Do NOT run `npm run transcribe:vo:<lc>`** unless the MP3 files themselves changed. Transcription is expensive (API) and unnecessary when only positions moved — local per-clip word offsets are still valid, only `abs_start` shifts.

3. **Audit scene alignments** against the refreshed CSV:
   - Scene `data-start` values anchored to VO word starts
   - Scene durations based on VO phrase ends (+ 1.5s crossfade tail)
   - Boom-pattern `wordTime` values in `animateTextReveal`, `animateEvolution`, `animateChannels`, etc.
   - Hard-coded absolute cues in `academy-kit.js` entrance tweens

4. **Re-align** using the usual formulas:
   - `scene.data-start = wordAbsStart` (for the scene's entry word)
   - `localOffset = wordAbsStart - sceneDataStart` (for inside-scene tweens)
   - `cardStart = wordAbsStart - 0.5` (boom pattern)

5. **Verify with a snapshot** at the affected moment before declaring done.

**Fast inspection:**
```bash
git diff --stat index.html          # which audio data-starts changed
npm run timing:vo:<lc>                    # regenerate cue files
rg "(ElevenCreative|ElevenAgents|...)" timing/voiceover-word-timing.csv
```

## Word-Cued Timing

Users often direct timing by spoken word ("start this when they say scale", "bring in the card on real conversations"). The VO timing files exist specifically to translate those directions. See `rules/animation-patterns.md` → "Word-Cued Visual Edits" for the full workflow. Short version:

- `timing/voiceover-word-timing.csv` has `abs_start`/`abs_end` for every word — already in composition-time seconds
- `localOffset = wordAbsStart - sceneDataStart` when timing an animation inside a scene
- Rerun `npm run timing:vo:<lc>` after any Studio audio-start change so the cue files match reality

## Two-Source-of-Truth Invariant

The composition's timing lives in three places — all must agree:

1. **`<section>` `data-start`/`data-duration`** in `index.html` (Studio reads these)
2. **`SCENES` array** in `assets/academy-kit.js` (GSAP timeline reads these)
3. **Hard-coded absolute cues** inside `fromToIf`/`toIf` calls (e.g., `22.88`, `85.63`, `169.38`) for bespoke per-scene choreography

Change any one → update the others. Running `npm run timing:vo:<lc>` after an audio-start change keeps the VO cue sheet in sync, but it does NOT update the `SCENES` array or hard-coded cues — you must edit those by hand.

## Composition Duration

```
TOTAL_DURATION = sum(scene_i.duration) - sum(crossfade_overlap_i)
```

With a 1.5s crossfade overlap between every pair of adjacent scenes, a composition with N scenes has `N - 1` overlaps. Example: 19 scenes with durations summing to ~210s and 18 overlaps of 1.5s each → total ~183s. The reference project (M01L01v6) totals 195.96s with less overlap discipline — match your spec, not blindly the reference.

Set `TOTAL_DURATION` in `academy-kit.js` and the root `<div data-duration>` in `index.html` to the same value.

## Render Pipeline

1. **Snapshots (fast)** — `npx hyperframes snapshot --at 2` and at representative mid-scene timestamps. Use for layout/typography spot-checks.
2. **Draft render (medium)** — `npm run render:draft:<lc>`. Use for timing/audio/export behavior checks.
3. **Standard render (slow)** — `npm run render:<lc>`. Final MP4.

Always lint before rendering:

```bash
npm run lint:<lc>
# or for info-level findings:
npm run validate:<lc>
```

## Font Loading

Fonts at `assets/fonts/` are embedded by the HyperFrames compiler automatically — just reference `font-family: 'KMR Waldenburg'` in `academy-kit.css`. No `@font-face` rules needed.

If a weight is missing, `npx hyperframes validate` surfaces a warning. Match the weight names used by the font files:

- `KMR-Waldenburg-Buch.otf` → weight 300 (Light/Book)
- `KMR-Waldenburg-Normal.otf` → weight 400 (Regular)
- `KMR-Waldenburg-Halbfett.otf` → weight 600 (Semibold)

## Project Bootstrap Checklist

When starting a new lesson build, run the scaffolding workflow in `rules/lesson-scaffolding.md`. In short:

1. Create `<DIR>/` (e.g., `M02L01/`) under the monorepo root with `index.html`, `meta.json` (id set), `hyperframes.json`, and an `assets/voiceover/<ID>/manifest.json` shell
2. Symlink `<DIR>/shared → ../shared` so brand assets (`shared/academy-kit.{css,js}`, `assets/brand-assets/`, `assets/fonts/`, `assets/logos/`, `assets/icons/`, `assets/generated/`) are reachable inside the lesson root
3. Append per-lesson npm scripts (`preview:<lc>`, `lint:<lc>`, `render:<lc>`, etc.) to root `package.json`
4. Create `assets/voiceover/<comp-id>/` with the VO MP3s and `manifest.json`
5. Update `scripts/transcribe-voiceover.sh`'s `audio_dir` to the new path
6. Write `index.html` with all scenes + VO clips
7. Replace `SCENES` array in `academy-kit.js` to match
8. Run the full validation loop: `npm run lint:<lc> && npm run timing:vo:<lc> && npx hyperframes snapshot --at 2`
