---
name: hyperframes-spec
description: "Build HyperFrames video spec files — scene-by-scene blueprints using the ElevenLabs two-mode visual system. Creates structured spec documents for any branded ElevenLabs video: product demos, training content, marketing videos, internal communications, lesson narration. Use when: 'draft a spec', 'create a video spec', 'spec out the video', 'plan the scenes', 'write a HyperFrames spec', 'add scenes', or any HyperFrames composition planning. The spec is implementation-agnostic markdown describing every scene (mode, layout, duration, VO line, content fields) and is the handoff document for the hyperframes-builder skill."
---

# ElevenLabs HyperFrames Spec Drafting Skill (v1)

This skill drafts structured spec files for ElevenLabs branded videos that get implemented in HyperFrames. These specs are the handoff document between content planning and the **hyperframes-builder** skill that turns them into HTML compositions.

## What a Spec File Is

A spec file is a markdown document that describes every scene in a video: what layout it uses, what mode it's in (Hero or Content), what text appears on screen, how long it lasts, and what VO line it accompanies. The hyperframes-builder skill reads this file and generates the `index.html`, GSAP timeline cues in `brand-kit.js` (or your project's equivalent), and any new CSS classes from it.

The spec is **NOT code**. It's a creative/structural document that maps content to visuals using the ElevenLabs two-mode design system.

The spec format mirrors the older Remotion spec format (which this skill replaces) — implementation-agnostic. So a Remotion spec from `remotion-spec` (deprecated) can be implemented by `hyperframes-builder` with minimal translation.

## How to Use This Skill

### When Drafting a New Spec

1. **Read the brief or script first.** The source document contains the VO, production notes, and objectives. The spec translates that content into scenes.
2. **Load the visual reference:** Read `references/brand-guidelines.md` for the ElevenLabs design system — colors, typography, animation patterns, scene template diagrams. Cross-reference `references/bundled-assets.md` for the brand asset inventory you can call out by name.
3. **Load an example spec** from a recent project to see how a completed spec looks.
4. **Draft the spec** following the template format below and save to the project's spec directory as `{ProjectCode}-spec.md` (e.g., `ProductDemo2026v1-spec.md`).

### When Updating an Existing Spec

1. Read the existing spec file.
2. Load the visual reference if you need to check layout options or component names.
3. Make the changes — add/remove/reorder scenes, update content, adjust durations.
4. Update the Scene Flow Summary table and total duration.

### When the Project Will Be Built in HyperFrames

If the spec is going into an existing shared HyperFrames monorepo, the **hyperframes-builder** skill should follow that repo's local scaffolding workflow and implement the scenes against the shared `shared/brand-kit.{css,js}` brand surface. The spec should call out the project id (e.g., `ProductDemo2026v1`) so the builder knows what to create.

For standalone branded videos, `hyperframes-builder` builds a standalone HyperFrames project using the shared brand kit assets in this plugin.

## Two-Mode Scene System

Every scene must be classified as either **Hero** or **Content** mode. This determines the entire visual treatment.

### Mode 1: Hero (Gradient Backgrounds)

Use for: title cards, openers, concept reveals, methodology scenes, outros, atmospheric moments, brand-forward moments.

| Property | Value |
|----------|-------|
| **Background** | Full-bleed image from `brand-assets/backgrounds/` with blur (~25px) + noise (7% opacity, overlay blend) + dark tint `rgba(0,0,0,0.12)` |
| **Text color** | WHITE (`#FFFFFF`) for headlines and body |
| **Cards** | Dark flat (default): `rgba(0,0,0,0.3)` bg, no blur, border `1px solid rgba(255,255,255,0.12)`, radius 14px, shadow `0 8px 32px rgba(0,0,0,0.3)`. |
| **Card text** | `#FFFFFF` for titles, `rgba(255,255,255,0.65)` for descriptions |
| **II Watermark** | WHITE at 25% opacity (use `.watermark.white` class) |
| **Decorative numbers** | `rgba(30,25,22,0.06)` |
| **Accent lines** | WHITE at 60% opacity |
| **Feel** | Atmospheric, immersive, brand-forward |

### Mode 2: Content (Light Backgrounds)

Use for: step sequences, code examples, detail content, numbered lists, checklists, comparisons, feature grids — anything the viewer needs to read and retain.

| Property | Value |
|----------|-------|
| **Background** | OFF_WHITE (`#FAFAFA`) solid |
| **Text color** | GRAPHITE (`#1E1916`) for headlines, DARK_GRAY (`#555555`) for body |
| **Cards** | Solid CREAM (`#F5F3F1`) with LIGHT_GRAY (`#E0DEDC`) borders |
| **Decorative numbers** | LIGHT_GRAY (`#E0DEDC`) |
| **II Watermark** | GRAPHITE at 35% opacity |
| **Feel** | Clean, readable, instructional |

### Choosing a Mode

- **Hero** when the moment is emotional, transitional, or brand-forward.
- **Content** when the viewer needs to read and retain information. Clarity over atmosphere.
- A typical video alternates: Hero (title) → Content (detail) → Hero (concept reveal) → Content (steps) → Hero (outro).

## Spec File Format

Every spec follows this structure:

```markdown
# {ProjectCode} — [Video Title]

**Project:** [project name or campaign]
**Version:** [number]
**Duration:** ~[total] seconds ([scene count] scenes)
**VO Pace:** ~145 words/min, conversational

## Video Summary
[2-3 sentences: what this video communicates, key takeaway]

## Scene Plan
### Scene 1 — Title Card
[scene block format below...]

### Scene N — Outro
[scene block format below...]

## Scene Flow Summary
[table of all scenes with mode, layout, duration, component]
```

### Scene Block Format

Each scene follows this exact table + content structure:

```markdown
### Scene [N] — [Scene Name]
| | |
|---|---|
| **Mode** | Hero / Content |
| **Layout** | [from Available Layouts] |
| **Component** | [existing scene-pattern name from hyperframes-builder, or blank for builder to decide] |
| **Duration** | [seconds — note this is in **seconds** for HyperFrames, not frames] |
| **Background** | [for Hero: background image filename; for Content: omit or "OFF_WHITE"] |
| **VO Line** | [the narration line this scene accompanies] |

**Content:**
- **Section Label:** `[1-3 words, uppercase]`
- **Headline:** `[2-6 words]`
- [additional fields per layout...]

**Notes:** [special behavior, active items, badges, background choice rationale, etc.]
```

### Why "seconds" not "frames"

HyperFrames uses **absolute seconds** for `data-start`/`data-duration`/GSAP positions. No frame math, no fps multiplications. A scene with `data-duration="14.48"` runs 14.48 seconds at any output fps.

This is one of the few places the HyperFrames spec format actively differs from a Remotion spec — Remotion specs typically express durations as frames at a target fps, and the Remotion builder converts. For HyperFrames specs, always express duration in seconds.

## Available Layouts (Quick Reference)

The hyperframes-builder skill recognizes these scene patterns. See `hyperframes-builder/rules/scene-patterns.md` for the full HTML templates each one produces.

### Hero Layouts

| Layout | Use For | Key Elements |
|---|---|---|
| `title-card` | Lesson/video opener | Brand lockup top-left, multi-line `hero-title` (small/big/small wrap), accent-line, hero-subtitle, bottom-bar, full-bleed background |
| `concept-reveal` | Single-statement moment | One large `.section-headline` centered over full-bleed background; optional sub-headline |
| `text-reveal-3` | "Three things" reveal sequence | Three `.dark-card` boxes appearing in sequence aligned to VO words |
| `evolution` | Three-card flow with arrows | Cards labeled (e.g., "Research → Platform → Agents") connected by `.card-arrow` SVGs |
| `channels` | Three-card category list | Three cards (e.g., "Voice / Chat / Messaging") + caption underneath |
| `welcome` | Pause-and-orient slide | Same as title-card layout but with a different small/big/small headline ("Welcome to / X / Course") |
| `pillars` | Three-product / three-platform | Three vertical cards, optional `.focus-badge` on the highlighted one |
| `split-detail` | One product/concept on the right + meta on the left | Right side has product banner + grid of features; left has section label, headline, body |
| `outro` | Closing/up-next slide | Mirrors title-card layout with "Up Next / [Topic] / Lesson 1.X" content |

### Content Layouts

| Layout | Use For | Key Elements |
|---|---|---|
| `feature-grid` | 4-card capability grid | Section label + headline + header rule + 4 cards in a row, each with icon + title + description |
| `module-map` / `timeline` | Curriculum overview | Numbered module list on the left, atmospheric image right (Hybrid mode also valid) |
| `quiz` | Knowledge check | Question card + 4 lettered options + cursor animation hitting the correct answer |
| `certification` / `pillar-detail` | Cert badge + sales copy | Left: cert kicker + headline + body + caption; Right: dark blue panel with shield image |
| `step-sequence` | Numbered "1, 2, 3" workflow | Numbered cards with descriptions, optionally with images |

If the brief calls for a layout not in this table, describe it in the Notes field — `hyperframes-builder` will figure out the implementation or ask for clarification.

## Related Skills (audio side)

When drafting a spec, you'll want to know what's possible on the audio side. These skills cover the ElevenLabs APIs that produce VO and music:

| Skill | Use during spec drafting |
|---|---|
| `text-to-speech` | Pick a voice approved for the current project, reference the `eleven_v3` audio-tag vocabulary (`[warm]`, `[confident]`, `[chuckle]`, `[pause]`, `[excited]`) when scripting VO lines for the spec. |
| `music` | Generate a music bed for the video. Use the corporate-explainer prompt for narration-bed branded videos (see `hyperframes-builder/rules/composition-patterns.md` → "Music bed"). |
| `speech-to-text` | If the spec adapts an existing audio file, transcribe it with Scribe v2 to get word-level cue times before designing the visual. |

The actual generation happens during build (`hyperframes-builder` invokes these via `scripts/generate-vo.mjs` and the music API), but knowing the constraints up front shapes the spec — e.g., voice choice affects pacing/wpm; music character affects scene tonality.

## Reference Files

| File | When to load |
|------|--------------|
| `references/brand-guidelines.md` | Colors, typography, asset paths, scene template diagrams — the visual reference |
| `references/brand-checklist.md` | Pre-publish brand QA checklist |
| `references/bundled-assets.md` | Brand asset inventory (backgrounds, voice orbs, icons, logos) — call out by exact filename |
| `references/chladni-patterns.md` | Atmospheric Chladni texture options for Hero scenes |

## Rules Directory

Load the file that matches what you're specifying:

| File | When to load |
|------|--------------|
| `rules/animations.md` | Specifying entrance/exit animations and easing |
| `rules/audio.md` | Music bed, audio clips, and audio routing |
| `rules/audio-visualization.md` | Audio-reactive visuals (waveforms, beat sync, frequency bars) |
| `rules/calculate-metadata.md` | Computing duration / fps / size from inputs |
| `rules/charts.md` | Data visualization (bar/line/pie chart specs) |
| `rules/compositions.md` | How a HyperFrames composition is structured (one root + scenes) |
| `rules/fonts.md` | Custom font loading (KMR Waldenburg + alternates) |
| `rules/images.md` | Image clips and animation |
| `rules/maps.md` | Map-based scenes |
| `rules/measuring-dom-nodes.md` | Measuring DOM elements at runtime for layout |
| `rules/measuring-text.md` | Text length / wrap measurement |
| `rules/parameters.md` | Composition parameters (props passed in at render) |
| `rules/sequencing.md` | Scene-to-scene sequencing and crossfade |
| `rules/subtitles.md` | Subtitles / captions / VTT |
| `rules/text-animations.md` | Text animation patterns (typewriter, marker sweep, etc.) |
| `rules/timing.md` | Timing fundamentals (seconds, durations, crossfades) |
| `rules/transitions.md` | Scene transitions (fades, wipes, shader transitions) |
| `rules/trimming.md` | Trimming source clips |
| `rules/videos.md` | Embedding video clips |
| `rules/voiceover.md` | VO chunking, manifest format, ElevenLabs voice selection |

Some rule files were copied from the Remotion spec skill verbatim because the capability is implementation-agnostic. A few may have a Remotion code example — treat those as conceptual references; the builder will translate to HyperFrames HTML+GSAP at implementation time.

## Common Mistakes to Avoid

1. **Specifying durations in frames** — HyperFrames uses seconds. Write `Duration: 14.48` not `Duration: 434f` or `Duration: 14480ms`.
2. **Listing a Remotion-only component name** in the Component field — like `WaveformSceneV2` is fine (it has a HyperFrames equivalent in `scene-patterns.md`); but `Lottie` or `ThreeFiber` aren't supported. Spec the visual you want, not the Remotion implementation.
3. **Forgetting the VO Line** for a content-driven scene — every scene with VO must have its line in the table so the builder can word-align card reveals (see `hyperframes-builder/rules/animation-patterns.md` → "Boom Pattern").
4. **Skipping mode classification** — every scene needs Hero or Content mode in the table. The builder uses this to pick text colors, card style, watermark color.
5. **Inventing a layout name** instead of using one from the Available Layouts table — only invent if the brief truly needs something new, and put a clear description in Notes.

## Project Context

This skill replaces `remotion-spec` (deprecated). The output spec format is intentionally similar so prior Remotion specs can be implemented in HyperFrames without rewriting.

After drafting a spec, hand it to **hyperframes-builder** which scaffolds the project (or lesson dir for course videos), drops in the `index.html` skeleton, and starts building scenes. For framework-specific patterns and CLI commands, the **hyperframes** and **hyperframes-cli** skills (also in this plugin) cover the runtime contract and CLI invocation.
