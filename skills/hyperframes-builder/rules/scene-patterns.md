# Scene Patterns for HyperFrames

HTML templates for every scene type in the v2 ElevenLabs visual system. All scenes:

- Live inside `<body>` inside the root `<div id="root" data-composition-id="main">`
- Are `<section>` elements with `class="clip scene noise"` and full `data-start`/`data-duration`/`data-track-index` + explicit `z-index`
- Carry a `.watermark` (white on Hero, default on Content/Hybrid)
- Never use inline scene-specific CSS — extend `assets/brand-kit.css`

## Root Composition Shell

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=1920, height=1080" />
    <title>ElevenLabs - M{XX}L{YY}v{Z} HyperFrames Port</title>
    <link rel="stylesheet" href="assets/brand-kit.css" />
    <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
    <script src="assets/brand-kit.js"></script>
  </head>
  <body>
    <div
      id="root"
      data-composition-id="main"
      data-start="0"
      data-duration="<total>"
      data-width="1920"
      data-height="1080"
    >
      <!-- scenes 1..N -->
      <!-- VO clips -->
      <!-- music bed -->
    </div>

    <script>
      window.__timelines = window.__timelines || {};
      window.__timelines["main"] = window.createBrandTimeline();
    </script>
  </body>
</html>
```

The standalone root does **not** use `<template>`. Only sub-compositions loaded via `data-composition-src` use `<template>` wrappers.

## Hero Scenes

### 1. Title Card (`WaveformSceneV2` equivalent)

```html
<section id="title-card" class="clip scene noise" data-start="0" data-duration="10" data-track-index="1" style="z-index: 99">
  <div class="scene-bg">
    <img src="assets/brand-assets/backgrounds/general/general-bg-01-blue-cyan-chladni.jpg" alt="" />
  </div>
  <div class="hero-content">
    <div class="brand-lockup" aria-label="ElevenLabs">
      <img class="brand-lockup-icon" src="assets/logos/elevenlabs-icon-white.svg" alt="" />
      <span>ElevenLabs</span>
    </div>
    <div>
      <h1 class="hero-title">Welcome to<br />ElevenLabs</h1>
      <div class="accent-line"></div>
      <div class="hero-subtitle">Module {X} - {Module Title}<br />Agent Builder Certification</div>
    </div>
  </div>
  <div class="bottom-bar"></div>
</section>
```

Notes:
- Title cards typically omit the watermark, relying on the brand lockup — match the spec.
- `.brand-lockup-icon` is the ElevenLabs II icon in white on the Brand lockup.
- Split title card variant: `WaveformSceneV2` lives full-gradient in v2 — do not recreate the v1 split. Keep full gradient.

### 2. Origin / One-Goal / Concept Reveal (`OriginStoryV2` equivalent)

```html
<section id="origin" class="clip scene noise" data-start="22.13" data-duration="8" data-track-index="3" style="z-index: 90">
  <div class="scene-bg">
    <img src="assets/brand-assets/backgrounds/chladni-closeup/closeup-07-pink-teal-blend.jpg" alt="" />
  </div>
  <div class="hero-content" style="justify-content: center">
    <div>
      <div class="section-label" data-rise>The Origin</div>
      <h2 class="tracking-title" style="font-size: 72px; font-weight: 300; line-height: 1.1; margin: 0 0 24px">One Goal</h2>
      <div class="accent-line"></div>
      <div class="hero-subtitle" data-rise>Build the most realistic AI voice<br />technology in the world.</div>
    </div>
  </div>
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

The `tracking-title` class gets a letter-spacing entrance (`letterSpacing: 0.45em → 0em`) paired with an x-translate — see `animation-patterns.md`.

### 3. Three-card Text Reveal (`TextRevealSceneV2` equivalent)

```html
<section id="text-reveal" class="clip scene noise" data-start="8.5" data-duration="14.48" data-track-index="2" style="z-index: 92">
  <div class="scene-bg"><img src="assets/brand-assets/backgrounds/chladni-closeup/<bg>.jpg" alt="" /></div>
  <div class="scene-panel">
    <header>
      <div class="section-label" data-rise>The Voice</div>
      <h2 class="section-headline" data-rise>The Most Natural Interface</h2>
      <div class="header-rule"></div>
    </header>
    <div class="card-row">
      <article class="dark-card feature-card" data-beat="1">
        <div class="card-num">01</div>
        <svg class="feature-icon" viewBox="0 0 48 48" fill="none"><!-- icon --></svg>
        <h3 class="card-title">Before screens.</h3>
      </article>
      <article class="dark-card feature-card" data-beat="2">
        <div class="card-num">02</div>
        <svg class="feature-icon" viewBox="0 0 48 48" fill="none"><!-- icon --></svg>
        <h3 class="card-title">Before keyboards.</h3>
      </article>
      <article class="dark-card feature-card" data-beat="3">
        <div class="card-num">03</div>
        <svg class="feature-icon" viewBox="0 0 48 48" fill="none"><!-- icon --></svg>
        <h3 class="card-title">And now it can scale.</h3>
      </article>
    </div>
  </div>
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

`data-beat="N"` drives per-beat entrance timing in `brand-kit.js` (see `animateTextReveal` in the reference implementation).

### 4. Three-card Flow with Arrows (`PlatformEvolutionV2` / `ChannelStripV2` equivalent)

```html
<section id="evolution" class="clip scene noise" data-start="28.63" data-duration="9" data-track-index="4" style="z-index: 89">
  <div class="scene-bg"><img src="assets/brand-assets/backgrounds/agents/<bg>.jpg" alt="" /></div>
  <div class="scene-panel">
    <header>
      <div class="section-label" data-rise>The Journey</div>
      <h2 class="section-headline" data-rise>From Research to Real Conversations</h2>
      <div class="header-rule"></div>
    </header>
    <div class="card-row">
      <article class="dark-card feature-card">
        <div class="card-num">01</div>
        <img class="feature-icon" src="shared/brand-assets/icons/white/<icon>.png" alt="" />
        <h3 class="card-title">Research</h3>
        <div class="card-description">AI voice models</div>
      </article>
      <div class="card-arrow">
        <svg width="32" height="32" viewBox="0 0 32 32">
          <path d="M6 16h20M20 10l6 6-6 6" fill="none" stroke="rgba(255,255,255,0.6)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </div>
      <article class="dark-card feature-card">
        <div class="card-num">02</div>
        <img class="feature-icon" src="shared/brand-assets/icons/white/<icon>.png" alt="" />
        <h3 class="card-title">Platform</h3>
        <div class="card-description">Full product suite</div>
      </article>
      <div class="card-arrow"><!-- same svg --></div>
      <article class="dark-card feature-card">
        <div class="card-num">03</div>
        <img class="feature-icon" src="shared/brand-assets/icons/white/<icon>.png" alt="" />
        <h3 class="card-title">Agents</h3>
        <div class="card-description">Real conversations</div>
      </article>
    </div>
  </div>
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

Always white icons on Hero. If using a product icon (tts/stt/convai), apply `filter: brightness(0) invert(1)` instead of grabbing a white variant that doesn't exist.

### 5. Welcome Card (`WelcomeCardV2` equivalent)

```html
<section id="welcome" class="clip scene noise" data-start="<s>" data-duration="12" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-bg"><img src="assets/brand-assets/backgrounds/gradient/Background<N>.jpg" alt="" /></div>
  <div class="hero-centered">
    <div class="section-label" data-rise>Welcome</div>
    <h2 class="hero-title" data-rise>Let's build your first agent.</h2>
    <div class="accent-line"></div>
    <div class="hero-subtitle" data-rise>This is where you start.</div>
  </div>
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

### 6. Three Platforms (`ThreePillarsSceneV2` equivalent)

Three cards, one labeled "YOUR FOCUS" (ElevenAgents is the cert focus). Use product banners if the spec calls for branded tops:

```html
<section id="pillars" class="clip scene noise" data-start="<s>" data-duration="10" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-bg"><img src="assets/brand-assets/backgrounds/gradient/<bg>.jpg" alt="" /></div>
  <div class="scene-panel">
    <header>
      <div class="section-label" data-rise>The Platforms</div>
      <h2 class="section-headline" data-rise>Three platforms. One voice stack.</h2>
      <div class="header-rule"></div>
    </header>
    <div class="card-row">
      <article class="dark-card feature-card">
        <div class="product-banner product-banner--creative"></div>
        <div class="product-kicker">CREATIVE</div>
        <h3 class="card-title">ElevenCreative</h3>
        <p class="product-description">The Studio. Voices for stories.</p>
      </article>
      <article class="dark-card feature-card feature-card--focus">
        <span class="focus-badge">YOUR FOCUS</span>
        <div class="product-banner product-banner--agents"></div>
        <div class="product-kicker">AGENTS</div>
        <h3 class="card-title">ElevenAgents</h3>
        <p class="product-description">The Agent Platform. Real conversations.</p>
      </article>
      <article class="dark-card feature-card">
        <div class="product-banner product-banner--api"></div>
        <div class="product-kicker">API</div>
        <h3 class="card-title">ElevenAPI</h3>
        <p class="product-description">The Developer Toolkit.</p>
      </article>
    </div>
  </div>
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

**Never write "pillars" in the visible copy when referring to the three products** — call them platforms. The scene id can stay `pillars` internally.

### 7. Outro (`OutroSceneV2` equivalent)

```html
<section id="outro" class="clip scene noise" data-start="<s>" data-duration="14" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-bg"><img src="assets/brand-assets/backgrounds/gradient/<bg>.jpg" alt="" /></div>
  <div class="hero-content">
    <img class="logo-icon logo-icon--white" src="assets/logos/ElevenLabs_Icon_White.svg" alt="" />
    <div class="section-label" data-rise>Up Next</div>
    <h2 class="hero-title" data-rise>{Next Lesson Title}</h2>
    <div class="accent-line"></div>
    <div class="hero-subtitle" data-rise>Lesson {X.Y+1}</div>
  </div>
  <img class="watermark white" src="assets/logos/ElevenLabs_Icon_White.svg#wm-<n>" alt="" />
</section>
```

The outro is the **only** scene allowed to fade elements out before the composition ends (final-scene exception).

## Content Scenes

### 8. Feature Grid (`FoundationDiagramV2` equivalent)

```html
<section id="research" class="clip scene noise content-mode" data-start="<s>" data-duration="18" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-panel wide">
    <header>
      <div class="section-label dark" data-rise>Research-Backed</div>
      <h2 class="section-headline" data-rise>Voice models built from research.</h2>
      <div class="header-rule"></div>
    </header>
    <div class="foundation-list">
      <div class="foundation-line"></div>
      <div class="foundation-pulse"></div>
      <div class="foundation-row">
        <div class="foundation-dot"></div>
        <article class="foundation-card">
          <h3 class="card-title">Research</h3>
          <p class="card-description">Voice models trained in-house.</p>
        </article>
      </div>
      <!-- more rows -->
    </div>
  </div>
  <img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg#wm-<n>" alt="" />
</section>
```

`.foundation-line` is the vertical spine connecting rows. `.foundation-pulse` is an animated dot traveling the line from top → bottom over ~2.65s (see `animation-patterns.md`).

### 9. Quiz / Knowledge Check (Content mode)

```html
<section id="knowledge-check" class="clip scene noise content-mode" data-start="<s>" data-duration="8" data-track-index="<n>" style="z-index: <n>">
  <div class="scene-panel">
    <header>
      <div class="section-label dark" data-rise>Knowledge Check</div>
      <h2 class="section-headline" data-rise>What is ElevenAgents?</h2>
      <div class="header-rule"></div>
    </header>
    <div class="quiz-card">
      <div class="quiz-option">The Studio</div>
      <div class="quiz-option">The Developer Toolkit</div>
      <div class="quiz-option correct">
        The Agent Platform
        <img class="checkmark" src="assets/icons/checkmark.png" alt="" />
      </div>
      <div class="quiz-option">A new model</div>
      <div class="cursor"></div>
      <div class="click-ring"></div>
    </div>
  </div>
  <img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg#wm-<n>" alt="" />
</section>
```

Quiz entrance sequence: card scales in → options stagger from below → cursor travels to the correct answer → click-ring pulses → checkmark pops in.

## Hybrid Scenes

### 10. Module Map / Timeline (`TimelineV4V2` equivalent)

```html
<section id="timeline" class="clip scene noise split-scene" data-start="<s>" data-duration="22" data-track-index="<n>" style="z-index: <n>">
  <div class="split-left">
    <div class="section-label dark" data-rise>The Curriculum</div>
    <h2 class="section-headline" data-rise>Eight modules. One certification.</h2>
    <div class="split-divider"></div>
    <p class="sub-headline">Each module builds on the last.</p>
  </div>
  <div class="split-right">
    <div class="bg"><img src="assets/brand-assets/backgrounds/gradient/<bg>.jpg" alt="" /></div>
    <div class="module-grid">
      <article class="module-item">
        <span class="number-label">01</span>
        <h4 class="card-title small">The Platform</h4>
        <p class="card-description">& Your First Agent</p>
      </article>
      <!-- modules 02..08 -->
    </div>
  </div>
  <img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg#wm-<n>" alt="" />
</section>
```

Watermark on hybrid scenes is **graphite**, not white (it sits on the white left side).

### 11. Pillar Detail / Certification (`PillarDetailSceneV2` equivalent)

```html
<section id="certification" class="clip scene noise split-scene" data-start="<s>" data-duration="10" data-track-index="<n>" style="z-index: <n>">
  <div class="split-left cert-copy">
    <div class="section-label dark" data-rise>The Credential</div>
    <h2 class="section-headline" data-rise>ElevenLabs Certified<br />ElevenAgent Builder</h2>
    <div class="split-divider"></div>
    <p class="sub-headline" data-rise>Build an agent end-to-end. Ship proof.</p>
  </div>
  <div class="split-right cert-right">
    <img class="bg" src="assets/brand-assets/backgrounds/gradient/<bg>.jpg" alt="" />
    <img class="cert-logo" src="assets/logos/elevenlabs-icon-white.svg" alt="" />
    <div class="shield-stage">
      <img src="assets/shield-trimmed.png" alt="" />
      <div class="shield-caption" data-rise>Agent Builder</div>
    </div>
  </div>
  <img class="watermark" src="assets/logos/ElevenLabs_Icon_Black.svg#wm-<n>" alt="" />
</section>
```

## Sub-Compositions (Optional)

If a scene is reused across lessons, factor it into `compositions/<name>.html` with a `<template>` wrapper and load via `data-composition-src`. Sub-composition contract:

```html
<template id="my-comp-template">
  <div data-composition-id="my-comp" data-width="1920" data-height="1080">
    <!-- content -->
    <style>
      [data-composition-id="my-comp"] { /* scoped styles */ }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
    <script>
      window.__timelines = window.__timelines || {};
      const tl = gsap.timeline({ paused: true });
      // tweens...
      window.__timelines["my-comp"] = tl;
    </script>
  </div>
</template>
```

Then in `index.html`:

```html
<div id="el-sub" data-composition-id="my-comp"
     data-composition-src="compositions/my-comp.html"
     data-start="100" data-duration="12" data-track-index="10"></div>
```

For the branded video port we've stayed in one `index.html` so far — only factor out a sub-composition when a pattern is used in 3+ lessons.

## Watermark Numbering

Each watermark `<img>` uses a unique URL fragment (`#wm-01`, `#wm-02`, ...) so the browser caches the same SVG once but the HyperFrames runtime can still treat each as a distinct DOM node for animation. Match the count to scene count.

## Z-Index Stacking

With a 1.5s crossfade between scenes, both the outgoing and incoming scene are in the DOM simultaneously during the overlap. Use monotonically decreasing `z-index` across scenes (e.g., scene 1 = 99, scene 2 = 98, ...) so the outgoing scene sits on top long enough to fade out before the next is revealed. Check the reference project's `index.html` — this is already the pattern.

## Content Sizing

All content containers fill the scene with `width: 100%; height: 100%; padding: Npx; box-sizing: border-box;`. **Never** use `position: absolute; top: Npx` for a content container — it overflows when content is taller than the remaining space. `position: absolute` is reserved for decoratives (watermark, accent lines, orbs, etc.).
