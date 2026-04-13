<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="brand-assets/logos/Logo-white.png">
    <img src="brand-assets/logos/Logo-black.png" alt="ElevenLabs" width="300" />
  </picture>
</p>

<h1 align="center">ElevenLabs Brand Kit</h1>

A Claude Code plugin for producing on-brand ElevenLabs content -- web apps, landing pages, marketing sites, and Remotion videos. Brand assets, design guidelines, and creative workflows in one package.

## What's Included

| Skill | Command | What It Does |
|-------|---------|-------------|
| **Asset Setup** | `/elevenlabs-brand-kit:asset-setup` | Download brand assets (~75MB), bootstrap a project, configure storage |
| **Brand** | `/elevenlabs-brand-kit:brand` | Enforce ElevenLabs brand guidelines on any content |
| **Branded Web** | `/elevenlabs-brand-kit:branded-web` | Build ElevenLabs-branded web experiences (HTML, CSS, React, Tailwind, shadcn/ui) |
| **Remotion Spec Builder** | `/elevenlabs-brand-kit:remotion-spec` | Draft scene-by-scene video blueprints with layouts, modes, backgrounds |
| **Remotion Builder** | `/elevenlabs-brand-kit:remotion-builder` | Generate React/TypeScript Remotion compositions from spec files |
| **Remotion Best Practices** | `/elevenlabs-brand-kit:remotion-best-practices` | Remotion API reference -- animations, transitions, audio, video, fonts, 3D |

### When to use which

- **"Build a branded web page"** → **Branded Web** -- CSS tokens, card variants, font loading, noise overlays, Tailwind/shadcn setup
- **"Is this on-brand?"** → **Brand** -- checks content against ElevenLabs brand guidelines
- **"Set up a new project"** → **Asset Setup** -- downloads brand assets and bootstraps the project
- **"Spec out a video"** → **Remotion Spec Builder** -- creates the video blueprint (scene layouts, modes, durations)
- **"Implement this spec"** → **Remotion Builder** -- generates the actual React code from a spec file

## Installation

### Via marketplace (recommended)

```
/plugin marketplace add jakerains/elevenlabs-brand-kit
```

```
/plugin install elevenlabs-brand-kit@elevenlabs-brand-kit
```

### Direct install (alternative)

```bash
claude /plugin install https://github.com/jakerains/elevenlabs-brand-kit
```

### Local development

```bash
git clone https://github.com/jakerains/elevenlabs-brand-kit.git
claude --plugin-dir ./elevenlabs-brand-kit
```

## Quick Start

### For Web Projects

1. Install the plugin
2. Run `/elevenlabs-brand-kit:asset-setup` -- choose "HTML / web project"
3. Start building with `/elevenlabs-brand-kit:branded-web` for CSS tokens, card variants, font loading, and layout guidance
4. Check brand compliance with `/elevenlabs-brand-kit:brand`

### For Video Projects

1. Install the plugin
2. Run `/elevenlabs-brand-kit:asset-setup` -- choose "Remotion video project"
3. Draft a spec with `/elevenlabs-brand-kit:remotion-spec`
4. Build it with `/elevenlabs-brand-kit:remotion-builder`

## Brand Assets

Brand assets (~75MB optimized) are distributed via [GitHub Releases](https://github.com/jakerains/elevenlabs-brand-kit/releases). The `asset-setup` skill downloads them and supports both project-local and central (`~/.elevenlabs-kit/`) storage.

**Included:**
- **52+ background images** -- gradients, agents-themed, Chladni close-ups, shared textures
- **400+ brand icons** -- cream, black, and white variants (130 each) + 10 product icons
- **8 voice orbs** -- metallic spheres in teal, purple, pink, coral, green, gold, hotpink, lavender
- **Logos** -- black + white wordmark, II symbol
- **4 KMR Waldenburg fonts** -- Book, Normal, Halbfett, Fett weights
- **Color tokens** (`color-tokens.json`) + **Brand Guidelines** (`BRAND_GUIDELINES.md`)
- **Visual catalog** (`index.html`) -- browse every asset with one-click copy buttons

## V2 Design System

The kit includes the full ElevenLabs V2 visual system, applicable to both web and video:

- **Hero mode** -- Gradient backgrounds, frosted BrandedCards, white text
- **Content mode** -- OFF_WHITE backgrounds, CREAM cards, GRAPHITE text
- **Hybrid mode** -- White left / gradient right split layouts
- **8 BrandedCard variants** -- darkflat, darkglass, glass, baseline, outline, pill, acrylic, gradientborder
- **Monochrome palette** -- Graphite, Off-White, Cream, Light-Gray, Mid-Gray; color comes from imagery

## Project Structure

```
elevenlabs-brand-kit/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── brand-assets/                # ~75MB optimized brand assets
│   ├── backgrounds/             # Gradients, agents, Chladni, textures
│   ├── icons/                   # 130 icons × 3 variants + product icons
│   ├── voice-orbs/              # 8 metallic orb images
│   ├── logos/                   # Wordmark + II symbol
│   ├── fonts/                   # KMR Waldenburg (4 weights)
│   ├── BRAND_GUIDELINES.md      # Official 2025 brand guidelines
│   ├── CLAUDE.md                # Asset inventory + usage guide
│   ├── color-tokens.json        # Color definitions
│   └── index.html               # Visual asset catalog
├── skills/
│   ├── asset-setup/             # Project bootstrap + asset download
│   ├── brand/                   # Brand guideline enforcement
│   ├── branded-web/             # Web development with brand system
│   │   └── rules/               # CSS tokens, Tailwind, shadcn, cards, backgrounds, typography
│   ├── remotion-best-practices/ # Remotion API patterns
│   │   └── rules/               # Animations, timing, transitions, audio, 3D, etc.
│   ├── remotion-builder/        # Code generation from specs
│   │   ├── rules/               # Brand system, animations, components, compositions
│   │   └── references/          # Component inventory, bundled assets
│   └── remotion-spec/           # Spec drafting
│       ├── assets/              # Spec templates and examples
│       ├── rules/               # Scene layouts, modes, backgrounds
│       └── references/          # Brand guidelines, asset inventory
└── README.md
```

## License

For ElevenLabs internal use only. This plugin is for authorized use by ElevenLabs employees. Brand assets, guidelines, fonts, and curriculum content are proprietary to ElevenLabs. No unauthorized use, reproduction, or distribution is permitted.
