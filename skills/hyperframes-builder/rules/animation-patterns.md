# Animation Patterns for HyperFrames

All motion lives in `assets/academy-kit.js` inside `createAcademyTimeline()`. The timeline is built paused; the HyperFrames runtime registers it and drives playback.

## Remotion → GSAP Conversion

Remotion's source animates with `useCurrentFrame()` + `spring()` at 30fps. In HyperFrames we use GSAP seconds. Straight conversions:

| Remotion (@30fps) | HyperFrames (GSAP) |
|-------------------|--------------------|
| `spring({ damping: 200 })` | ease `"power2.out"` / `"power3.out"` — never bouncy |
| `spring({ damping: 200, stiffness: 100 })` | `"power3.out"` with 0.55–0.70s duration |
| `TRANSITION_FRAMES = 45` | **1.5s crossfade** — built by `fadeScene(tl, scene)` (fade in 0.55s, fade out 0.75s, overlap = scene tail of 0.85s) |
| `ENTER_DELAY = 35` | **~1.17s entrance delay** — use `scene.start + 0.38` as the first entrance offset |
| Exit fade `[durationInFrames - 40, durationInFrames - 10]` | `tl.to(sel, { opacity: 0, duration: 0.75, ease: "power2.inOut" }, scene.start + scene.duration - 0.85)` |
| Stagger 8–20f | `stagger: 0.08–0.2` |
| Stagger 15f (card entrance) | `stagger: 0.11` |
| Stagger 4f (list items) | `stagger: 0.05` |

## The `SCENES` Array

`assets/academy-kit.js` exports no ES module; it wraps an IIFE that exposes `window.createAcademyTimeline`, `window.__academyScenes`, and `window.__academyTotalDuration`. The shape:

```js
(function () {
  const SCENES = [
    { id: "title-card", start: 0, duration: 10 },
    { id: "text-reveal", start: 8.5, duration: 14.48 },
    { id: "origin", start: 22.13, duration: 8 },
    // ...
  ];
  const TOTAL_DURATION = 195.96;

  // helpers + createAcademyTimeline below

  window.__academyScenes = SCENES;
  window.__academyTotalDuration = TOTAL_DURATION;
  window.createAcademyTimeline = createAcademyTimeline;
})();
```

`SCENES[i].start` + `SCENES[i].duration` MUST match the `<section id="SCENES[i].id">`'s `data-start` + `data-duration` in `index.html`. Any drift desyncs animation from scene visibility.

## Helpers (from the reference implementation)

```js
function sceneSelector(scene) {
  return "#" + scene.id;
}

function fromToIf(tl, selector, fromVars, toVars, position) {
  if (document.querySelector(selector)) {
    tl.fromTo(selector, fromVars, toVars, position);
  }
}

function toIf(tl, selector, vars, position) {
  if (document.querySelector(selector)) {
    tl.to(selector, vars, position);
  }
}
```

Use `fromToIf` / `toIf` instead of raw `tl.fromTo` / `tl.to` so missing selectors don't throw — this makes the timeline safe during iteration.

## Scene Fade (Crossfade)

Every scene gets a `fadeScene(tl, scene)` call. This is the crossfade engine — do not try to animate `.scene-bg` separately on exit.

```js
function fadeScene(tl, scene) {
  const sel = sceneSelector(scene);
  // Fade IN
  tl.fromTo(sel, { opacity: 0 }, { opacity: 1, duration: 0.55, ease: "power2.out" }, scene.start);
  // Fade OUT — 0.85s before scene end, over 0.75s
  tl.to(sel, { opacity: 0, duration: 0.75, ease: "power2.inOut" }, scene.start + scene.duration - 0.85);
}
```

The 0.85s tail + 0.75s fade-out means the outgoing scene reaches 0 opacity ~0.1s before its slot ends. If the next scene's `data-start` overlaps by ~1.5s (the v2 transition spec), the crossfade overlaps cleanly.

**Rule:** Never add `gsap.to(element, { opacity: 0 })` to an individual element before the scene's crossfade. The scene `<section>` fade-out handles the exit for everything inside it. The final scene is the only exception — it may fade individual elements out in the last 0.5s (fade-to-black effect).

## Base Entrance Template

```js
function baseEntrances(tl, scene) {
  const sel = sceneSelector(scene);
  const start = scene.start + 0.38;  // ENTER_DELAY equivalent (35f @ 30fps ≈ 1.17s; use 0.38 for crossfade alignment)
  const useCustomContentTiming = scene.id === "text-reveal" || scene.id === "research";

  // Rise entrances (labels, headlines, subheads)
  fromToIf(tl, sel + " [data-rise]",
    { opacity: 0, y: 26 },
    { opacity: 1, y: 0, duration: 0.62, ease: "power3.out", stagger: 0.11 },
    start);

  // Pop entrances (badges, icons that should scale in)
  fromToIf(tl, sel + " [data-pop]",
    { opacity: 0, scale: 0.9 },
    { opacity: 1, scale: 1, duration: 0.55, ease: "back.out(1.3)", stagger: 0.1 },
    start + 0.05);

  // Divider lines expand from left
  fromToIf(tl,
    sel + " .header-rule, " + sel + " .accent-line, " + sel + " .dark-accent, " + sel + " .bottom-bar, " + sel + " .split-divider",
    { scaleX: 0 },
    { scaleX: 1, duration: 0.7, ease: "power2.out", stagger: 0.05 },
    start + 0.2);

  // Card entrances (skip if the scene does custom card timing)
  if (!useCustomContentTiming) {
    fromToIf(tl,
      sel + " .dark-card, " + sel + " .module-item, " + sel + " .grid-feature, " + sel + " .foundation-row, " + sel + " .badge-card",
      { opacity: 0, y: 34 },
      { opacity: 1, y: 0, duration: 0.58, ease: "power3.out", stagger: 0.09 },
      start + 0.25);
  }
}
```

## Split Scene Entrance

```js
function animateSplitScene(tl, id, start) {
  const sel = "#" + id;
  // Background image reveal
  fromToIf(tl, sel + " .split-right .bg",
    { opacity: 0, scale: 1.05 },
    { opacity: 1, scale: 1, duration: 0.75, ease: "power2.out" },
    start + 0.25);
  // Left-side text
  fromToIf(tl, sel + " .split-left [data-rise]",
    { opacity: 0, y: 18 },
    { opacity: 1, y: 0, duration: 0.6, ease: "power3.out", stagger: 0.12 },
    start + 0.38);
  // Right-side content slides in from the right
  fromToIf(tl, sel + " .feature-grid, " + sel + " .module-grid",
    { opacity: 0, x: 24 },
    { opacity: 1, x: 0, duration: 0.7, ease: "power3.out" },
    start + 0.42);
}
```

## Orchestration

Inside `createAcademyTimeline()`:

```js
function createAcademyTimeline() {
  const tl = gsap.timeline({ paused: true });

  // Set initial state for scenes + dividers (transform origins, opacity 0)
  gsap.set(".scene", { opacity: 0 });
  gsap.set(".header-rule, .accent-line, .dark-accent, .bottom-bar, .split-divider",
    { transformOrigin: "left center" });

  // Generic per-scene fade + entrance
  SCENES.forEach((scene) => {
    fadeScene(tl, scene);
    baseEntrances(tl, scene);
  });

  // Title card custom entrances (lockup + hero-title + subtitle stagger)
  fromToIf(tl, "#title-card .academy-lockup",
    { opacity: 0, y: 16 },
    { opacity: 1, y: 0, duration: 0.55, ease: "power3.out" },
    1.1);
  fromToIf(tl, "#title-card .hero-title",
    { opacity: 0, y: 30 },
    { opacity: 1, y: 0, duration: 0.7, ease: "power3.out" },
    1.5);
  fromToIf(tl, "#title-card .hero-subtitle",
    { opacity: 0, y: 14 },
    { opacity: 1, y: 0, duration: 0.62, ease: "power3.out" },
    1.85);

  // Tracking-title (letter-spacing unfurl + x translate)
  fromToIf(tl, "#origin .tracking-title",
    { opacity: 0, letterSpacing: "0.45em", x: 18 },
    { opacity: 1, letterSpacing: "0em", x: 0, duration: 0.85, ease: "power3.out" },
    22.88);  // 22.13 (scene.start) + 0.75s offset

  // Scene-specific choreography
  animateTextReveal(tl);
  animateResearch(tl);
  ["creative", "api", "agents", "timeline"].forEach((id) => {
    const scene = SCENES.find((item) => item.id === id);
    animateSplitScene(tl, id, scene.start);
  });

  // Foundation pulse traveling down the research spine
  fromToIf(tl, "#research .foundation-pulse",
    { opacity: 0, y: 0 },
    { opacity: 0.95, y: 640, duration: 2.65, ease: "power1.inOut" },
    95.96);
  toIf(tl, "#research .foundation-pulse",
    { opacity: 0, duration: 0.35, ease: "power1.out" },
    98.35);

  // Quiz choreography
  fromToIf(tl, "#knowledge-check .quiz-card",
    { opacity: 0, scale: 0.94 },
    { opacity: 1, scale: 1, duration: 0.55, ease: "power3.out" },
    169.38);
  fromToIf(tl, "#knowledge-check .quiz-option",
    { opacity: 0, y: 16 },
    { opacity: 1, y: 0, duration: 0.42, ease: "power3.out", stagger: 0.08 },
    169.98);
  fromToIf(tl, "#knowledge-check .cursor",
    { opacity: 0, x: 700, y: 230, scale: 1 },
    { opacity: 1, x: 0, y: 0, duration: 0.85, ease: "power2.inOut" },
    171.63);
  toIf(tl, "#knowledge-check .cursor",
    { scale: 0.82, duration: 0.08, ease: "power2.out" },
    172.49);
  toIf(tl, "#knowledge-check .cursor",
    { scale: 1, duration: 0.16, ease: "power2.out" },
    172.57);
  fromToIf(tl, "#knowledge-check .click-ring",
    { opacity: 0.6, scale: 0.3 },
    { opacity: 0, scale: 1.25, duration: 0.5, ease: "power2.out" },
    172.49);
  fromToIf(tl, "#knowledge-check .quiz-option.correct .checkmark",
    { opacity: 0, scale: 0.75 },
    { opacity: 1, scale: 1, duration: 0.35, ease: "back.out(1.5)" },
    172.68);

  return tl;
}
```

## Scene-Specific Choreography Examples

### Text Reveal (three-beat card stagger)

```js
function animateTextReveal(tl) {
  const sceneStart = 8.5;
  const beats = [
    { beat: "1", cardDelay: 1.67 },
    { beat: "2", cardDelay: 3.67 },
    { beat: "3", cardDelay: 11.5 }
  ];

  beats.forEach((item) => {
    const sel = '#text-reveal [data-beat="' + item.beat + '"]';
    const cardStart = sceneStart + item.cardDelay;
    const textStart = cardStart + 0.82;

    fromToIf(tl, sel,
      { opacity: 0, y: 40 },
      { opacity: 1, y: 0, duration: 0.58, ease: "power3.out" },
      cardStart);
    fromToIf(tl, sel + " .feature-icon",
      { opacity: 0, scale: 0.85 },
      { opacity: 0.72, scale: 1, duration: 0.45, ease: "power2.out" },
      cardStart + 0.32);
    fromToIf(tl, sel + " .card-title",
      { opacity: 0, y: 24 },
      { opacity: 1, y: 0, duration: 0.62, ease: "power3.out" },
      textStart);
  });
}
```

Each beat has an absolute time inside the scene — the VO script aligns with these moments. The `cardDelay` values come from the lesson spec's VO beat timing.

### Research Foundation (vertical line + staggered rows)

```js
function animateResearch(tl) {
  const sceneStart = 91.63;

  fromToIf(tl, "#research .foundation-line",
    { scaleY: 0, opacity: 0 },
    { scaleY: 1, opacity: 1, duration: 0.7, ease: "power2.out" },
    sceneStart + 1.35);
  fromToIf(tl, "#research .foundation-row",
    { opacity: 0, x: 20 },
    { opacity: 1, x: 0, duration: 0.58, ease: "power3.out", stagger: 1.33 },
    sceneStart + 1.67);
  fromToIf(tl, "#research .foundation-card",
    { opacity: 0, y: 10 },
    { opacity: 1, y: 0, duration: 0.58, ease: "power3.out", stagger: 1.33 },
    sceneStart + 1.75);
}
```

Note the large `stagger: 1.33` — rows reveal one per VO beat, not in rapid succession.

## Rules

1. **Opacity + transforms only.** Never animate `display`, `visibility`, `width`, `height`, or media `src`. Wrap non-animatable elements in a div that IS animatable.
2. **Never animate the same property on the same element from multiple timelines simultaneously.**
3. **No `repeat: -1`.** Calculate finite repeats from scene duration: `repeat: Math.ceil(duration / cycleDuration) - 1`.
4. **No `async` / `setTimeout` / `Promise` inside the timeline factory.** The HyperFrames capture engine reads `window.__timelines` synchronously after page load.
5. **Offset the first entrance by 0.1–0.3s inside each scene.** Content that appears at exactly `scene.start` lands during the crossfade overlap and looks jumpy.
6. **Vary easings.** Use ≥3 different eases per scene for choreography variety (e.g., `power3.out` for headlines, `back.out(1.3)` for badges, `power2.inOut` for divider expansions).
7. **Use `gsap.set()` only on elements that exist at page load** (the `.scene` containers, transform origins). For scene-internal elements, use `tl.set(selector, vars, timePosition)` inside the timeline at/after the scene's `data-start`.
8. **Entrance-only per scene (except final).** No element-level exit fades before a crossfade — the scene fade-out handles it. The outro scene may fade individual elements out in the last 0.5s.
9. **Apply `[data-rise]` / `[data-pop]` attributes in HTML to opt elements into `baseEntrances`.** This avoids bespoke tweens for every headline/label — the base template handles them.

## When to Use `gsap.timeline` vs. Single Tweens

All animations live in one master timeline built by `createAcademyTimeline`.not create additional `gsap.timeline` instances — the HyperFrames runtime registers exactly one timeline per composition-id.

If a scene needs a sub-sequence (e.g., the quiz cursor flow), order the tweens on the master timeline using absolute time positions. Nested timelines are not needed for this project's complexity.

## Word-Cued Visual Edits

Users often direct timing by spoken word: *"start this when they say scale"*, *"bring in the card on real conversations"*, *"hold until production-ready systems"*. The VO timing pipeline (`npm run timing:vo:<lc>`) exists specifically to translate those directions into exact composition seconds.

Workflow:

1. **Find the phrase context** in the readable cue sheet:
   ```bash
   rg -n "real conversations|scale|production-ready" timing/voiceover-cues.md
   ```
2. **Find the exact word timing** in the CSV when you need precision:
   ```bash
   rg -n "scale|real|conversations" timing/voiceover-word-timing.csv
   ```
3. **Use `abs_start` / `abs_end`** from `timing/voiceover-word-timing.csv` as composition-time seconds. These values already include each audio clip's `data-start` offset from `index.html` — no addition needed.
4. **Moving a whole scene or clip?** Set its `data-start` to the target `abs_start` and adjust `data-duration` or following scene starts to keep the overlap discipline.
5. **Timing an animation inside an existing scene?** Convert the word time to a scene-local offset:
   ```
   localOffset = wordAbsStart - sceneDataStart
   ```
   Example: the `text-reveal` scene starts at `8.5s`. the user wants the third card to land when VO says "scale", which has `abs_start = 20.0`. The GSAP cardDelay is `20.0 - 8.5 = 11.5` — this is exactly the `cardDelay: 11.5` value used in `animateTextReveal` for `beat: "3"`.
6. **Entrance vs. exit alignment:**
   - For entrances, align to the word's `abs_start`.
   - For exits or holds, align to the word or phrase `abs_end`, then add a small beat (0.2–0.5s) if the visual needs breathing room.
7. **Disambiguating repeated words:** If a word appears more than once, pick by `clip_id` + surrounding cue text + scene context, not the first match. The CSV's `chunk_id` column points to the source VO clip.
8. **Order of operations** when changing either audio or visuals:
   - If Studio audio starts changed → rerun `npm run timing:vo:<lc>` before trusting the cue sheet
   - If visual timing changed → run `npm run validate:<lc>` and take a snapshot near the target time to confirm

**Why this matters:** Spoken-word direction is the common language between the script review and the motion built here. Always land the visual on the word, not the other way around — the VO is the rhythm, the animation serves it.

## Boom Pattern (Card-Row Word Sync)

When the user describes a card-row scene with phrases like **"boom boom boom"**, **"align the boxes to the words"**, **"card X hits on word Y"** — this is the canonical pattern for card-reveal scenes (text-reveal, evolution, channels, and any future three-box scene that mirrors VO keywords).

The "boom" is the moment the card lands on its word. Everything about the pattern serves that single percept.

### Recipe

1. **Identify each card's on-screen keyword** — the word in the card title that appears as the VO plays (e.g. "Research" / "Platform" / "Agents", "Voice" / "Chat" / "Messaging", "Before screens." / "Before keyboards." / "And now it can scale.").

2. **Find the matching VO word** in `timing/voiceover-word-timing.csv`. Use the **first occurrence within the scene's composition-time window**, not the first occurrence in the file.

   ```bash
   # Fast keyword sweep for a given VO clip
   rg "^vo-02," timing/voiceover-word-timing.csv \
     | awk -F',' '$4=="word" {print $5,$9}' \
     | grep -iE "research|platform|agents"
   ```

3. **Animate each card as ONE unit.** One `fromToIf` per card, targeting the whole `<article>` via `'#{scene} [data-beat="N"]'`.NOT add separate tweens for `.feature-icon` / `.card-title` / `.card-description` — those break the "card arrives with its keyword" percept, because `immediateRender` puts the title at opacity 0 until its own tween plays, so when VO says the word you see an empty-titled card.

4. **Default card entrance:**
   - `cardStart = wordTime - 0.5`
   - `from: { opacity: 0, y: 40 }`
   - `to:   { opacity: 1, y: 0, duration: 0.58, ease: "power3.out" }`

   At this setting the card is ~90% settled by the moment the word is spoken — arrives on the word, not after.

5. **Arrows / dividers between cards** anchor on narrative transition verbs, not filler words:
   - Research → Platform: arrow on "became"
   - Platform → Agents: arrow on "powers"
   - Default: `arrowStart = wordTime - 0.15`, 0.5s power2.out, `{opacity: 0, x: -12} → {opacity: 1, x: 0}`

6. **Caption / trailing line** (e.g. "At a scale no human team could match alone.") anchors on the first word of that phrase: `captionStart = firstWordTime - 0.3`.

7. **Skip `baseEntrances` card stagger** — add the scene id to the `useCustomContentTiming` list at the top of `baseEntrances()` in `academy-kit.js`. Otherwise the generic 90ms `.dark-card` stagger fires alongside your custom timing and they fight.

8. **Add HTML hooks**: `data-beat="1/2/3"` on each `<article class="dark-card feature-card">` and `data-arrow="1/2"` + `class="card-arrow"` on arrow divs (if the scene has arrows). Remove `data-rise` from any element you're custom-animating so `baseEntrances` doesn't double-up.

### Canonical shape of a scene animate function

```js
function animate<Scene>(tl) {
  // VO-XX "<quoted line from the cue sheet>"
  // Each card = one unit. Title/icon/number all arrive together ON the word.
  const beats = [
    { beat: "1", wordTime: <abs_start>, cardOffset: -0.500 }, // "<word>"
    { beat: "2", wordTime: <abs_start>, cardOffset: -0.500 }, // "<word>"
    { beat: "3", wordTime: <abs_start>, cardOffset: -0.500 }  // "<word>"
  ];
  const arrows = [ // optional — omit if scene has none
    { arrow: "1", wordTime: <abs_start> }, // "<transition-verb>"
    { arrow: "2", wordTime: <abs_start> }
  ];

  beats.forEach((item) => {
    const sel = '#<scene-id> [data-beat="' + item.beat + '"]';
    fromToIf(tl, sel,
      { opacity: 0, y: 40 },
      { opacity: 1, y: 0, duration: 0.58, ease: "power3.out" },
      item.wordTime + item.cardOffset);
  });

  arrows.forEach((item) => {
    const sel = '#<scene-id> [data-arrow="' + item.arrow + '"]';
    fromToIf(tl, sel,
      { opacity: 0, x: -12 },
      { opacity: 1, x: 0, duration: 0.5, ease: "power2.out" },
      item.wordTime - 0.15);
  });

  // If the scene has a caption:
  fromToIf(tl, "#<scene-id> .caption",
    { opacity: 0, y: 14 },
    { opacity: 1, y: 0, duration: 0.6, ease: "power3.out" },
    <captionFirstWord> - 0.3);
}
```

Then call `animate<Scene>(tl)` inside `createAcademyTimeline()`.

### Shorthand the user may use

|||
|-----------|-----|
| "boom on research, platform, agents in evolution" | Apply pattern to `#evolution` cards anchored on those words |
| "align the channels cards to the VO" | Find keywords in card titles, grep CSV, apply pattern |
| "the Platform card should hit on 'platform' not 'that platform'" | Change wordTime to the first in-scene occurrence |
| "it's still off — card enters after the word" | Check cardOffset isn't 0 or positive — should be `-0.5` |
| "title is missing when the word is said" | Remove separate `.card-title` tween; animate whole article as one |

### Canonical examples already in the codebase

- `animateEvolution` — 3 cards on "research" (30.489), "platform" (31.689), "agents" (34.449); arrows on "became" (31.130), "powers" (34.010)
- `animateChannels` — 3 cards on "voice" (38.230), "chat" (38.929), "messaging" (39.509); caption on "at" (40.409)
- `animateTextReveal` — 3 cards on "screens" (10.899), "keyboards" (12.399), phrase-start "and" (19.979)

Mirror these when the scene is a three-box card-row. For different scene types (foundation diagrams, quiz choreography, timeline grids), use the patterns in `scene-patterns.md`.

## Deterministic Motion Only

No `Math.random()`, no `Date.now()`, no `performance.now()` to seed values. Pseudo-random offsets (e.g., noise particle positions) must use a seeded PRNG like mulberry32:

```js
function mulberry32(seed) {
  return function () {
    let t = (seed += 0x6d2b79f5);
    t = Math.imul(t ^ (t >>> 15), t | 1);
    t ^= t + Math.imul(t ^ (t >>> 7), t | 61);
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}
const rng = mulberry32(42);  // same seed → same animation every render
```
