# brand-kit.css Class Inventory

Complete reference of CSS classes available in `assets/brand-kit.css`. Use these before inventing new ones. Extend the kit when new layout is genuinely needed — don't inline scene-specific styles in `index.html`.

The reference implementation is at `~/Projects/hyperframes/assets/brand-kit.css`. When porting a new lesson, copy this file verbatim into the new project and extend it.

## Scene Shell

| Class | Purpose |
|-------|---------|
| `.scene` / `.scene.clip` | Top-level `<section>` — full 1920×1080 stacking context, relative positioning |
| `.noise` | Apply to `<section>`; enables `.noise::before` SVG fractal noise at 7% opacity with `mix-blend-mode: overlay` |
| `.scene-bg` | Hero scene background wrapper — full-bleed, `z-index: 0` |
| `.scene-bg img` / `.scene-bg-image` | Background image; `object-fit: cover`, typically with CSS blur + filter |
| `.scene-bg-image--blue-orange` | Variant modifier for specific tonal treatments |
| `.scene-bg::after` | Dark tint overlay (`rgba(0,0,0,0.12)`) on Hero backgrounds |
| `#root` / `[data-composition-id="main"]` | Root composition sizing/positioning |

## Content Containers

| Class | Purpose |
|-------|---------|
| `.hero-content` | Centered/flow-driven content region for Hero scenes — default `justify-content: flex-start` |
| `.hero-centered` | Centered content for welcome/outro — `justify-content: center; text-align: center` |
| `.scene-panel` | Content-mode or Hero-mode main content container with standard padding |
| `.scene-panel.wide` | Wider variant for long diagrams (e.g., `#research` foundation list) |
| `.split-scene` | Two-column hybrid layout: 45% left + 55% right |
| `.split-left` | Left (white/graphite) side of a split scene |
| `.split-right` | Right (gradient/Hero) side of a split scene |
| `.split-right .bg` | Background image wrapper inside `.split-right` |
| `.split-divider` | Vertical or horizontal accent line between split halves |

## Typography

| Class | Purpose |
|-------|---------|
| `.hero-title` | 78px, weight 300 — title card + welcome + outro headlines |
| `.hero-subtitle` | 27px, weight 300 — subtitles under hero titles |
| `.section-label` | 16px, weight 600 uppercase +10% tracking — Hero-mode label (white/60%) |
| `.section-label.dark` | Content-mode variant (MID_GRAY) |
| `.section-headline` | 52px, weight 300 — main scene headline |
| `.sub-headline` | 27px, weight 300 — secondary line under headline |
| `.tracking-title` | Large title with letter-spacing unfurl entrance (e.g., "One Goal") |
| `.number-label` | Small monospace-like numbered label |

## Logo & Watermark

| Class | Purpose |
|-------|---------|
| `.logo-wordmark` | Full ElevenLabs wordmark |
| `.logo-icon` | ElevenLabs II icon (default graphite) |
| `.logo-icon--white` | White variant for Hero scenes |
| `.brand-lockup` | "II ElevenLabs" lockup for title cards |
| `.brand-lockup-icon` | Icon inside the brand lockup |
| `.watermark` | Bottom-right corner 32px from edges, ~20px height, graphite at 35% opacity |
| `.watermark.white` | White variant at 25% opacity (Hero scenes) |

## Decoration

| Class | Purpose |
|-------|---------|
| `.accent-line` | Short horizontal accent under hero titles (scales from 0 on entrance) |
| `.bottom-bar` | Full-width bottom bar on title card (scales from left on entrance) |
| `.header-rule` | Full-width 1px divider under `.section-headline` |
| `.dark-accent` | Dark tinted accent line for Content scenes |

## Cards

| Class | Purpose |
|-------|---------|
| `.dark-card` | Hero-mode flat translucent black card (`rgba(0,0,0,0.3)`, 1px rgba white border, no backdrop-filter) |
| `.feature-card` | Modifier for card-row cards with icon + title + description |
| `.feature-card.left` | Left-aligned variant |
| `.card-num` | Large decorative number (100–140px, weight 300, low opacity) |
| `.card-title` | Card title (34–38px, weight 400) |
| `.card-title.small` | Smaller title variant |
| `.card-description` | Card body/description text |
| `.card-label` | Small uppercase label above card title |
| `.card-divider` | Internal horizontal rule inside a card |
| `.caption` | Small caption text below imagery |
| `.feature-icon` | Icon inside a card (typically 48px, white on Hero) |
| `.icon-box` | Framed icon container |

## Card Row / Grid

| Class | Purpose |
|-------|---------|
| `.card-row` | Flex row of cards, centered with gap 28px |
| `.feature-grid` | Grid of features (used in split-right) |
| `.grid-feature` | Item inside `.feature-grid` |
| `.grid-feature svg` / `.grid-feature img` / `.grid-feature .grid-icon` | Icon slots inside grid feature |
| `.grid-feature .grid-icon` | Explicit grid icon class |
| `.grid-icon--tts` / `.grid-icon--convai` | Product icon variants |
| `.grid-feature h3` / `.grid-feature p` | Grid feature text styling |
| `.focus-badge` | Small "YOUR FOCUS" pill on a focus card (absolute positioned bottom-left) |
| `#agents .focus-badge` | Agent-specific focus badge styling (existing id in reference) |

## Product Banners

| Class | Purpose |
|-------|---------|
| `.product-banner` | Base banner (thin colored strip at top of a product card) |
| `.product-banner--creative` | Orange accent for ElevenCreative |
| `.product-banner--agents` | Blue accent for ElevenAgents |
| `.product-banner--api` | Monochrome for ElevenAPI |
| `.product-kicker` | Small uppercase kicker above product name |
| `.product-description` | Product tagline inside a product card |

## Hero Moment (`#diff-intro`-style)

| Class | Purpose |
|-------|---------|
| `.centered-big` | Large centered statement (e.g., "Three things make it different") |

## Research / Foundation (`#research`-style)

| Class | Purpose |
|-------|---------|
| `.foundation-list` | Container for vertical spine diagram |
| `.foundation-line` | Vertical spine (scales from 0 on entrance) |
| `.foundation-pulse` | Animated dot traveling down the spine |
| `.foundation-row` | Horizontal row: dot + card |
| `.foundation-dot` | Dot marker on the spine |
| `.foundation-card` | Card attached to a foundation row |

## Quiz / Knowledge Check (`#knowledge-check`-style)

| Class | Purpose |
|-------|---------|
| `.quiz-card` | Outer quiz card container |
| `.quiz-option` | Individual answer option |
| `.quiz-option.correct` | Styled green/success state |
| `.checkmark` | Checkmark icon on correct option |
| `.cursor` | Animated cursor traveling to the correct option |
| `.click-ring` | Pulse ring on click (opacity + scale animation) |

## Certification (`#certification`-style)

| Class | Purpose |
|-------|---------|
| `.cert-copy` | Left-side text container on certification scene |
| `.cert-right` | Right-side shield + logo container |
| `.cert-right img.bg` | Background image inside cert-right |
| `.cert-logo` | ElevenLabs logo above the shield |
| `.shield-stage` | Shield + caption stage |
| `.shield-caption` | Text label under the shield |

## Module Grid / Timeline

| Class | Purpose |
|-------|---------|
| `.module-grid` | Grid of module items on Timeline/Hybrid scenes |
| `.module-item` | Individual module card |
| `.badge-card` | Small badge-style card used in module grid |

## Entrance Attributes (read by brand-kit.js)

These are not CSS classes but `data-*` attributes that opt elements into `baseEntrances()`:

| Attribute | Entrance behavior |
|-----------|-------------------|
| `data-rise` | Rise entrance: `{ opacity: 0, y: 26 } → { opacity: 1, y: 0 }`, `power3.out`, 0.62s, stagger 0.11s |
| `data-pop` | Pop entrance: `{ opacity: 0, scale: 0.9 } → { opacity: 1, scale: 1 }`, `back.out(1.3)`, 0.55s, stagger 0.1s |
| `data-beat="N"` | Used on three-beat cards (`#text-reveal`); consumed by `animateTextReveal()` for per-beat timing |

## When to Add a New Class

Add a new CSS class to `brand-kit.css` when:

1. The same visual treatment appears on 2+ scenes
2. Scene-specific layout would otherwise require inline `style=""` on more than a handful of elements
3. A new spec introduces a pattern not covered by the inventory above (e.g., a new kind of product showcase)

**Don't** add a new class when:

- The styling is one-off for a single scene (one inline style attribute is fine)
- You're about to name it `.scene-N-foo` — that's a sign it should be scene-scoped inline or an id-prefixed selector
- The style is already covered by an existing class with a small adjustment — use a modifier (`.dark-card--variant`) instead

## When to Update This Reference

If you extend `brand-kit.css`, update this file in the skill so future lessons can find the new classes. Treat it as documentation that earns its keep — stale class references waste more time than missing ones.
