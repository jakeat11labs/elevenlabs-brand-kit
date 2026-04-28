# ElevenLabs Brand System for HyperFrames

This is the HyperFrames translation of the Remotion v2 two-mode brand system. Same colors, same typography, same rules — implemented as CSS classes in `assets/brand-kit.css` rather than React components.

## Color Palette

```css
/* Foundation — monochrome */
--graphite:    #1E1916;  /* Text on Content, graphic marks. NEVER backgrounds. */
--off-white:   #FAFAFA;  /* Content scene backgrounds */
--cream:       #F5F3F1;  /* Card fills (solid on Content, frosted on Hero) */
--light-gray:  #E0DEDC;  /* Borders, dividers, decorative numbers (Content) */
--mid-gray:    #999999;  /* Labels, section labels (Content) */
--dark-gray:   #555555;  /* Body text, descriptions (Content) */
--white:       #FFFFFF;  /* Text + watermark on Hero */
--black:       #000000;  /* Use sparingly */
--success:     #2F9E64;  /* Correct quiz states */
```

**Color proportion rule:** The layout stays mostly monochrome. Color arrives via imagery (Hero background photos), never as applied UI swatches. Gray shades should not be mixed with color accents.

## Typography

```css
font-family: 'KMR Waldenburg', 'Inter', 'Helvetica Neue', sans-serif;
```

Weights:
- **300 (Book/Light)** — large text, refined headlines
- **400 (Normal)** — card names, secondary content
- **600 (Halbfett)** — labels, taglines, badges

Never use **700** for headlines. The font ships with Halbfett (600) and Fett (700), but only Book/Normal/Halbfett are part of the official hierarchy.

### Typographic Scale

| Role | Size | Weight | Tracking | Hero color | Content color |
|------|------|--------|----------|------------|---------------|
| Title card headline (`.hero-title`) | 78px | 300 | -3% | WHITE | GRAPHITE |
| Scene headline (`.section-headline`) | 52px | 300 | -2% | WHITE | GRAPHITE |
| Section label (`.section-label`) | 16px | 600 uppercase | +10% | `rgba(255,255,255,0.6)` | MID_GRAY |
| Sub-headline (`.sub-headline`) | 27px | 300 | 0 | WHITE | DARK_GRAY |
| Card name (`.card-title`) | 34–38px | 400 | -1% | `#151515` (frosted) / `#FFFFFF` (darkflat) | GRAPHITE |
| Card tagline (uppercase) | 18px | 600 | +4% | `rgba(30,25,22,0.4)` / `rgba(255,255,255,0.7)` | MID_GRAY |
| Card/body text (`.card-description`) | 20–24px | 300 | 0 | `#666666` / `rgba(255,255,255,0.65)` | DARK_GRAY |
| Hero words (e.g., LEARN/BUILD/PROVE) | 76–80px | 400 | +2% | WHITE | GRAPHITE |
| Decorative numbers (`.card-num`) | 100–140px | 300 | 0 | `rgba(30,25,22,0.06)` on Hero cards | LIGHT_GRAY |

**Weight principle:** Light (300) for large text. Heavier (400–600) for small text and labels. Never use `vw`/`vh` font sizes — they break at non-1920×1080 previews.

## Font Loading

Fonts live at `assets/fonts/` and are embedded automatically by the HyperFrames compiler — just reference `font-family: 'KMR Waldenburg'` in CSS. No `@font-face` boilerplate needed. If a weight is missing, `npx hyperframes validate` surfaces a warning.

## Spacing

- **Scene content padding:** `80px 120px` (on `.scene-panel` / `.hero-content`)
- **Card grid padding (`.card-row`):** `60px 100px`
- **Card internal padding (`.dark-card` / `.feature-card`):** `40px 36px`
- **Card gap:** 28px (flex gap on `.card-row`)
- **Card border radius:** 14px (Hero) / 16px (Content)
- **Split scenes:** 45% left / 55% right (use `.split-scene` with `.split-left` / `.split-right`)

## Mode 1 — Hero

Used for title cards, module openers, concept reveals, methodology, outros, atmospheric moments.

### Structure

```html
<section id="<id>" class="clip scene noise"
         data-start="<s>" data-duration="<s>" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-bg">
    <img src="assets/brand-assets/backgrounds/<category>/<file>.jpg" alt="" />
  </div>

  <div class="hero-content">
    <div class="section-label" data-rise>The Origin</div>
    <h2 class="section-headline" data-rise>One Goal</h2>
    <div class="accent-line"></div>
    <div class="hero-subtitle" data-rise>
      Build the most realistic AI voice<br />technology in the world.
    </div>
  </div>

  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

### Visual rules

| Property | Value |
|----------|-------|
| Background | Full-bleed `<img>` inside `.scene-bg`. `object-fit: cover`, CSS `filter: blur(~25px)` and dark tint via `.scene-bg::after { background: rgba(0,0,0,0.12); }` |
| Noise overlay | `.noise::before` — SVG fractal noise at 7% opacity, `mix-blend-mode: overlay` |
| Headline text | WHITE (`#FFFFFF`) |
| Labels | `rgba(255,255,255,0.6)` |
| Accent lines (`.accent-line`, `.bottom-bar`) | WHITE at 60% opacity |
| Cards | `.dark-card` flat translucent black (`rgba(0,0,0,0.3)`, `1px solid rgba(255,255,255,0.12)`, radius 14px, **no backdrop blur** to avoid grid artifacts) |
| Card titles | `#FFFFFF` |
| Card descriptions | `rgba(255,255,255,0.65)` |
| Decorative numbers | `rgba(30,25,22,0.06)` |
| Watermark | `.watermark.white` — full opacity (never dim the II on Hero) |

### Backgrounds by category

All under `assets/brand-assets/backgrounds/`:

| Category | Feel | When to use |
|----------|------|-------------|
| `agents/` | Green, teal, blue | Agent-related content, platform scenes |
| `chladni-closeup/` | Mixed warm/cool, diverse natural | Concept reveals, emotional moments |
| `general/` | Blue/cyan with Chladni overlay | Title cards, generic Hero |
| `gradient/` | 16 atmospheric gradients | Clean, versatile default for any Hero |
| `creative/` | Coral sunset | Creative/warm accent |

Pick by tonal fit, not literal content match. The compositing pipeline (image → blur → tint → noise → content) normalizes the visual.

### Dark-card variants (when you need more than `.dark-card`)

The Remotion `BrandedCard` component exposes 8 variants. Translate to CSS modifiers on `.dark-card`:

| Variant | Background | Blur | Border | When to use |
|---------|-----------|------|--------|-------------|
| `darkflat` (default) | `rgba(0,0,0,0.3)` | none | `1px solid rgba(255,255,255,0.12)` | **Default for all Hero cards.** No blur artifacts in dense grids. |
| `darkglass` | `rgba(0,0,0,0.3)` | `blur(20px)` | same | Isolated single card. Avoid in dense grids. |
| `glass` | `rgba(255,255,255,0.08)` | `blur(40px)` | `1px solid rgba(255,255,255,0.2)` | Deep glassmorphism where background should bleed through |
| `baseline` | `rgba(245,243,241,0.88)` | `blur(24px)` | `1px solid rgba(255,255,255,0.12)` | Frosted cream (original v2). Dark text on frosted card. |
| `outline` | transparent | none | `1px solid rgba(255,255,255,0.25)` | Border only, no fill |
| `pill` | `rgba(255,255,255,0.12)` | `blur(30px)` | `1px solid rgba(255,255,255,0.18)` | Radius 24px for soft pill shapes |
| `acrylic` | `rgba(245,243,241,0.45)` | `blur(12px)` | `1px solid rgba(255,255,255,0.15)` | Fluent-inspired with noise texture |
| `gradientborder` | `rgba(0,0,0,0.15)` | `blur(20px)` | gradient stroke | Shimmer gradient border accent |

Add as modifier classes: `class="dark-card dark-card--glass"` etc. Extend `brand-kit.css` with the modifier; don't inline the style block.

## Mode 2 — Content

Used for step sequences, code examples, detail content, checklists, feature grids.

### Structure

```html
<section id="<id>" class="clip scene noise content-mode"
         data-start="<s>" data-duration="<s>" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-panel">
    <header>
      <div class="section-label dark" data-rise>The Journey</div>
      <h2 class="section-headline" data-rise>From Research to Real Conversations</h2>
      <div class="header-rule"></div>
    </header>
    <!-- content area: .card-row / .feature-grid / .foundation-list / etc. -->
  </div>

  <img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg#wm-<n>" alt="" />
</section>
```

### Visual rules

| Property | Value |
|----------|-------|
| Background | OFF_WHITE (`#FAFAFA`) solid — no `.scene-bg` image |
| Noise overlay | Same 7% SVG fractal noise (apply `.noise` class) |
| Headlines | GRAPHITE (`#1E1916`) |
| Body | DARK_GRAY (`#555555`) |
| Labels (`.section-label.dark`) | MID_GRAY (`#999999`) |
| Cards | Solid CREAM (`#F5F3F1`) with `1px solid #E0DEDC` borders |
| Dividers (`.header-rule`, `.card-divider`) | LIGHT_GRAY (`#E0DEDC`) |
| Decorative numbers | LIGHT_GRAY (`#E0DEDC`) |
| Watermark | default `.watermark` (graphite) at 35% opacity |

### Content card pattern

```html
<article class="solid-card feature-card">
  <div class="card-num">01</div>
  <img class="feature-icon" src="shared/brand-assets/icons/black/<icon>.png" alt="" />
  <h3 class="card-title">Research</h3>
  <div class="card-description">AI voice models</div>
</article>
```

Focus card (e.g., "Your Focus" in ThreePillars):

```html
<article class="solid-card feature-card feature-card--focus">
  <span class="focus-badge">YOUR FOCUS</span>
  <!-- ... -->
</article>
```

`.feature-card--focus` styling: `border: 2px solid #1E1916`. Badge: GRAPHITE bg, OFF_WHITE text, 11px weight 600 uppercase tracking +8%, padding `5px 12px`, radius 6px.

## Mode 3 — Hybrid (White/Gradient Split)

Used for module maps, timelines, PillarDetail scenes. White left (text/narrative) + gradient right (visual system).

### Structure

```html
<section id="<id>" class="clip scene noise split-scene"
         data-start="<s>" data-duration="<s>" data-track-index="<n>" style="z-index: <n>">
  <div class="split-left">
    <div class="section-label dark" data-rise>The Path</div>
    <h2 class="section-headline" data-rise>How You'll Learn</h2>
    <div class="split-divider"></div>
    <p class="sub-headline">Every module builds on the last.</p>
  </div>
  <div class="split-right">
    <div class="bg">
      <img src="assets/brand-assets/backgrounds/gradient/<file>.jpg" alt="" />
    </div>
    <!-- module grid / foundation list / feature grid with dark-card children -->
    <div class="module-grid">
      <!-- ... -->
    </div>
  </div>

  <img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg#wm-<n>" alt="" />
</section>
```

### Visual rules

- Left 45% is Content-mode surface (OFF_WHITE, GRAPHITE text)
- Right 55% is Hero-mode surface (gradient background + `.dark-card` children)
- Watermark sits on the left (Content) side — use graphite variant, **not** white
- PillarDetail scenes: description text inside `.split-left` uses fixed `height: 120px` for consistent centering across variants
- "Your Focus" badge on a right-side card: position absolute at bottom-left, NOT in flex flow

## Persistent Elements — Required on Every Scene

### Watermark

Bottom-right, 32px from edges, ~20px icon height. Title card currently uses 42px deliberately — match the spec.

- Hero: `<img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg">` — full opacity
- Content/Hybrid: `<img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg">` — opacity 0.35

**Never** dim the watermark on title/welcome/outro scenes.

### Noise overlay

Add `class="noise"` to every `<section class="scene">`. Driven by `.noise::before` in `brand-kit.css` (SVG fractal noise at 7% opacity, `mix-blend-mode: overlay`).

### Chladni patterns (optional)

Subtle texture (3–6% opacity). Use for research/voice/origin scenes where extra atmosphere is wanted. Overlay as a separate absolutely-positioned `<div>` inside the scene.

## Scene Structure (Content/Hybrid)

Every Content scene follows this vertical order:

1. `.section-label` (16px uppercase) at top
2. `.section-headline` (52px, weight 300) below
3. `.header-rule` full-width 1px divider (scales from 0 → 1 on entrance)
4. Content area filling the remaining space
5. `.watermark` bottom-right

Every Hero scene typically:
1. `.scene-bg` full-bleed
2. `.hero-content` centered or justify-start
3. Content: label → headline → `.accent-line` → subtitle / cards
4. `.bottom-bar` (optional) for title/outro scenes
5. `.watermark.white` bottom-right

## Brand Terminology (Never Slip)

- **"ElevenLabs"** — one word, capital E, capital L. Never "Eleven Labs", "Elevenlabs", "elevenlabs", "ELEVENLABS", "IIElevenLabs", "11ElevenLabs".
- **Three platforms** — ElevenCreative (The Studio, orange accent), ElevenAgents (The Agent Platform, blue, cert focus), ElevenAPI (The Developer Toolkit, monochrome). **Never "pillars"** when referring to the three products.
- **Three pillars of trust** (distinct concept) — Research-backed, Configurable, Trusted. OK to call these "pillars."
- **"ElevenLabs"** — the subtitle on title cards.
- **Credential name** — "ElevenLabs Certified ElevenAgent Builder".
- **II symbol** — visual only. Never typed in content text. Never use "I I" or "11" to recreate it.
- **Origin story** — ElevenLabs was founded as a research company to solve **dubbing**. Agents came later. Never write "built to solve the conversation volume problem."
- **First person** — use "we"/"our"/"us" for ElevenLabs. Never third person ("they", "their platform", "ElevenLabs offers").

## Product Icon Rule (MUST apply to every HyperFrames project)

Product icon PNGs in `assets/icons/` and `shared/brand-assets/icons/product/` (`convai.png`, `tts.png`, `stt.png`, `dubbing.png`, `studio.png`, etc.) ship as **black line art on transparent**. On Hero-mode dark/gradient cards they render as invisible silhouettes unless whitened.

**Two CSS patterns exist for icons in `.grid-feature` cards** — both must be whitened:

```css
.grid-feature img {
  filter: brightness(0) invert(1);   /* applies to <img src="...png"> */
}

.grid-feature .grid-icon {
  filter: brightness(0) invert(1);   /* applies to <div class="grid-icon grid-icon--convai"> */
}
```

Inline `<svg>` icons inside `.grid-feature` use `stroke="currentColor"` and pick up `color: var(--white)` automatically — no filter needed.

**Alternative (preferred when available):** use a pre-whitened icon from `shared/brand-assets/icons/white/` instead of filtering. ~130 white-variant icons exist. Only apply the filter when no white variant is available for the specific product icon you need.

**On Content-mode scenes** (OFF_WHITE bg, solid cream cards): do NOT apply the white filter — icons should be GRAPHITE/black. Use `shared/brand-assets/icons/black/` or leave the native black PNG.

**Detection:** if a Hero scene's 2×2 grid has 3 icons that read clearly and 1 that looks like a dark square or empty slot, the odd one is an `<img>` missing the filter while its siblings are inline SVGs.

## Common Mistakes

1. **`backdrop-filter` on dense card grids** — 4+ cards close together compositing blur layers produces artifacts. Use `darkflat` (the default), not `darkglass`.
2. **`flex: 1` on cards** — cards hug their content, centered vertically with `align-items: center` on the parent flex container. Never stretch cards vertically.
3. **Cream/dark text on a translucent Hero card** — if using the `darkflat` variant, card text is white. Only `baseline` / `acrylic` variants take dark text.
4. **Black or cream icons on Hero scenes** — always use white variants from `shared/brand-assets/icons/white/`, or apply `filter: brightness(0) invert(1)` to product icons (tts, stt, convai, etc.). See "Product Icon Rule" above — `.grid-feature img` MUST have the filter or PNG product icons render as black silhouettes on dark cards.
5. **Generic blue/purple gradients in place of brand imagery** — use the copied `brand-assets/backgrounds/` library. Don't CSS-gradient your way out of picking a background.
6. **Viewport-scaled font sizes (`vw`, `vh`)** — breaks at non-1920×1080 preview/snapshot sizes. Use fixed `px`.
7. **Inline scene-specific styles in `index.html`** — extend `brand-kit.css` instead. The composition stays readable in Studio.
