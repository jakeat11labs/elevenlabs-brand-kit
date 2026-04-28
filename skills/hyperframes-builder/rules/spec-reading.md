# Reading HyperFrames Specs

Specs come from one of two sources:

- **`hyperframes-spec`** (this plugin) — drafts spec markdown for general ElevenLabs branded videos (product demos, marketing, internal comms, etc.). Output: `{ProjectCode}-spec.md` with the standard Hero/Content scene blocks.
- **`lesson-spec-builder`** (separate skill, only relevant for ElevenLabs lessons) — produces Remotion-format specs at `10 - Remotion/specs/M{XX}L{YY}v{Z}v2-spec.md`. Designed for the legacy Remotion builder but also drive HyperFrames builds — the spec format is implementation-agnostic.

Both flavors follow the same Hero/Content scene block format. For course videos, the canonical reference spec is `10 - Remotion/specs/M01L01v5v2-spec.md`.

This document explains how to translate either flavor of spec into HyperFrames code.

## Spec Structure

Every spec follows this shape:

```markdown
# M{XX}L{YY}v{Z}v2 — [Lesson Title]

**Module:** {number} — {module name}
**Lesson:** {number} — {lesson name}
**Version:** {Z}v2
**Duration:** ~{total} seconds ({scene count} scenes)
**VO Pace:** ~145 words/min, conversational

---

## Video Summary
[2–3 sentences]

## v2 Design System Reference
[Hero/Content scene split table]

## Sizing Reference
[Standard dimensions]

## Scene Plan

### Scene 1 — {title}
| | |
|---|---|
| **Layout** | Full Gradient |
| **Component** | `WaveformSceneV2` |
| **Mode** | **Hero** |
| **Background** | `brand-assets/backgrounds/general/[image].jpg` |
| **Duration** | 8–10 seconds |
| **VO Beat** | [First line of VO] |
| **Screengrab** | No |

### Scene 2 — {title}
...
```

## Field-by-Field Translation (Remotion → HyperFrames)

### `Component`

The spec names a Remotion React component (e.g., `WaveformSceneV2`). Map these to HyperFrames scene patterns (full templates live in `rules/scene-patterns.md`):

| Remotion component | HyperFrames equivalent | Scene type |
|--------------------|------------------------|------------|
| `WaveformSceneV2` | `#title-card` section, full gradient, `.hero-content` with brand lockup | Title card |
| `OutroSceneV2` | `#outro` section, full gradient, `.logo-icon--white` + "Up Next" | Outro |
| `WelcomeCardV2` | `#welcome` section, `.hero-centered` with hero title | Centered hero |
| `OriginStoryV2` | `#origin`-style section, `.hero-content` with `.tracking-title` | Concept reveal |
| `TextRevealSceneV2` | `.scene-panel` + `.card-row` of 3 `.dark-card.feature-card[data-beat]` | Three-beat cards |
| `PlatformEvolutionV2` / `ChannelStripV2` | `.scene-panel` + `.card-row` of 3 `.dark-card` with `.card-arrow` separators | Three-step flow |
| `ThreePillarsSceneV2` | `.scene-panel` + 3 `.dark-card` with `.product-banner--{creative,agents,api}`, one with `.feature-card--focus` + `.focus-badge` | Three platforms |
| `PillarDetailSceneV2` | `.split-scene` with `.split-left` text + `.split-right` frosted card grid | Hybrid pillar detail |
| `TimelineV4V2` | `.split-scene` with `.module-grid` on right | Hybrid module map |
| `FoundationDiagramV2` | `.scene-panel.wide` + `.foundation-list` with `.foundation-line`, `.foundation-pulse`, `.foundation-row` | Content vertical spine |
| `BrandedCard` | `.dark-card` (with variant modifier: `.dark-card--glass`, `.dark-card--baseline`, etc.) | Frosted/flat card |
| `GradientBackground` | `.scene-bg` + `<img>` + `.noise` class on scene | Hero bg treatment |
| `IIWatermark` | `.watermark` (`color="white"` → `.watermark.white`) | Persistent mark |
| `NoiseOverlay` | `.noise` class on `<section>` (drives `.noise::before`) | Ambient texture |
| `FullBleedPlaceholder` | Empty `<section class="scene">` with placeholder color/class — marked in HTML comment `<!-- PLACEHOLDER: replace in Premiere -->` | Screen-recording slot |
| `ConversationLoop` / `DecisionMatrix` / `RecapGrid` / etc. | Content-mode scene with custom `.scene-panel` layout — extend `brand-kit.css` | Custom Content scene |

If the spec names a component not in this table, it's one of the Remotion "v1 → v2 upgrade" scenes or a new scene pattern. Approach:

1. Read the Remotion source at `~/Projects/remotion/src/components/v2/<ComponentName>.tsx` to understand the structure and props
2. Translate the JSX tree to equivalent HTML using existing `brand-kit.css` classes where possible
3. Only introduce new CSS classes if genuinely new layout is required — prefer extending what's there

### `Mode`

- **Hero** → Full-bleed `.scene-bg` image + white text + `.dark-card` children + `.watermark.white`
- **Content** → No `.scene-bg`; OFF_WHITE surface + GRAPHITE text + solid cream cards + `.watermark` (graphite)
- **Hybrid** → `.split-scene` with `.split-left` (Content surface) + `.split-right` (Hero surface); watermark is `.watermark` (graphite)

Full visual rules per mode: `rules/brand-system.md`.

### `Background`

Path is relative to `public/brand-assets/` in Remotion; in HyperFrames it's relative to `assets/brand-assets/`. Translate:

```
brand-assets/backgrounds/general/general-bg-01-blue-cyan-chladni.jpg
↓
<img src="assets/brand-assets/backgrounds/general/general-bg-01-blue-cyan-chladni.jpg" alt="" />
```

If the spec references a background that doesn't exist in `assets/brand-assets/`, copy it from `~/Projects/remotion/public/brand-assets/` — the HyperFrames project's asset library mirrors the Remotion one.

### `Duration`

Straight number of seconds. Set both:
- `<section data-duration="<s>">` in `index.html`
- Matching `SCENES[i].duration` in `assets/brand-kit.js`

If the spec says "8–10 seconds", pick based on VO length + 1.5s crossfade margins at each end.

### `VO Beat`

The first line or key phrase of the VO for the scene. Cross-reference with the lesson's `_VO.md` file in `01 - Certification Program/Lessons/Module {X}/L{X}.{Y}/` to find the full chunk text. This maps to the `<audio id="vo-NN">` whose MP3 transcribes to that text.

The VO chunks are numbered in order (`01-*.mp3` through `NN-*.mp3`); use the `manifest.json` `chunks[].scenes` array to confirm which chunk covers which scene.

### `Screengrab: Yes/No`

Indicates whether the scene should be exportable as a reference still. For HyperFrames this maps to "snap this timestamp after build":

```bash
npx hyperframes snapshot --at <s>
```

Take snapshots of every `Screengrab: Yes` scene as part of the output checklist. `Screengrab: No` scenes are atmospheric/transitional — no need to snapshot them individually.

## Scene Plan Reading Order

Work top-down through the spec's Scene Plan section:

1. **Scene 1** → write the first `<section>` in `index.html`, add `SCENES[0]` to `brand-kit.js`
2. **Scene 2** → write the second `<section>` with `data-start = SCENES[0].start + SCENES[0].duration - 1.5` (the crossfade overlap), add `SCENES[1]`
3. Continue...

Running `data-start` math:
```
SCENES[i].start = SCENES[i-1].start + SCENES[i-1].duration - 1.5
```

(1.5s = the v2 transition overlap.)

Exception: scenes adjacent to placeholders use hard cuts (no overlap) — the next scene starts exactly at the placeholder's end.

## Placeholder Handling

When the spec includes `FullBleedPlaceholder` scenes (for screen recordings to be added in Premiere):

- Give the placeholder scene a visually obvious placeholder style (e.g., diagonal stripes with "REPLACE IN PREMIERE" text) so the user can spot it during review
- **Hard cut** into and out of the placeholder — no fade. The motion scenes adjacent should end/start cleanly at the placeholder boundary.
- Adjust adjacent motion scene durations to include **2–4s extra trim room** for Premiere handle
- Document the placeholder in an HTML comment: `<!-- PLACEHOLDER: Agent Builder UI walkthrough, ~60s, replace in Premiere -->`

## Sizing Reference

The spec includes a "Sizing Reference" section; treat these as the source of truth for layout values. Typical v2 values:

- Content padding: `80px 120px`
- Card internal padding: `40px 36px`
- Card gap: `28px`
- Card radius: `14px` (Hero) / `16px` (Content)
- Split left: 45%, split right: 55%
- Watermark: 32px from edges, ~20px height (title card may use larger per spec)

If the spec overrides any of these, follow the spec. Otherwise match `brand-kit.css` defaults.

## Verification Loop

After building the first few scenes, sanity-check against the spec:

```bash
npx hyperframes lint
npx hyperframes snapshot --at <scene-1-mid>
npx hyperframes snapshot --at <scene-2-mid>
```

Compare snapshots to the Remotion reference build (`~/Projects/remotion/out/`) for the same lesson. Delta should be:
- Same typography and color
- Same layout proportions
- Different motion character is expected (GSAP eases ≠ Remotion springs by feel, but the timing lands on the same words)

If the visual deltas are large, revisit the scene pattern — you're probably missing a CSS class or using the wrong background.

## When the Spec Is Ambiguous

Ask the user, don't guess. Specs are a living document — if a scene description says "three cards" but doesn't specify mode or layout, flag it rather than inventing. Particularly for:

- Custom visualization scenes (waveforms, particle flows, data visualizations)
- Any scene referencing "new v2 scene" without a working Remotion implementation
- Screengrab vs. placeholder ambiguity
- New backgrounds, icons, or voice-orb assets that haven't been mirrored into `assets/brand-assets/`

## Module Title Reference

Always use landing-page names. If the spec quotes a different module name, flag it for the user:

1. The Platform & Your First Agent
2. Voice & AI Foundations
3. Prompt Engineering
4. Knowledge & Tools
5. Advanced Architecture
6. Deployment
7. Testing & Evaluation
8. Production Operations
- Certification Exam — Prove It (separate, paid)

The current spec for Module 1 Lesson 1 is `M01L01v5v2-spec.md`. Future lessons follow `M{XX}L{YY}v{Z}v2-spec.md`.
