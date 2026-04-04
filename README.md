# ElevenLabs Academy Plugin for Claude Code

A Claude Code plugin that provides the complete toolkit for producing ElevenLabs Academy certification videos using Remotion.

## What's Included

| Skill | Command | What It Does |
|-------|---------|-------------|
| **Setup** | `/elevenlabs-academy:asset-setup` | Bootstrap a new project — installs Remotion, downloads brand assets, verifies setup |
| **Remotion Builder** | `/elevenlabs-academy:remotion-builder` | Build Remotion compositions from spec files. The main code generation skill. |
| **Remotion Spec** | `/elevenlabs-academy:remotion-spec` | Draft structured spec files that describe lesson videos scene by scene |
| **Remotion Best Practices** | `/elevenlabs-academy:remotion-best-practices` | Remotion API reference — animations, transitions, audio, video, fonts, 3D |
| **Lesson Builder** | `/elevenlabs-academy:lesson-builder` | Generate full lesson content — VO scripts, Remotion specs, text companions |
| **Lesson Author** | `/elevenlabs-academy:lesson-author` | Author VO scripts and lesson narratives |
| **Brand** | `/elevenlabs-academy:brand` | Enforce ElevenLabs brand guidelines on any content |
| **Project Context** | `/elevenlabs-academy:project-context` | Curriculum structure, module info, credential details |

## Installation

### From GitHub (recommended)

```bash
claude /plugin install https://github.com/jakerains/elevenlabs-academy-plugin
```

### Local development

```bash
git clone https://github.com/jakerains/elevenlabs-academy-plugin.git
claude --plugin-dir ./elevenlabs-academy-plugin
```

## Quick Start

1. Install the plugin
2. Run `/elevenlabs-academy:asset-setup` to bootstrap your Remotion project with brand assets
3. Draft a spec with `/elevenlabs-academy:remotion-spec`
4. Build it with `/elevenlabs-academy:remotion-builder`

## Brand Assets

Brand assets (backgrounds, icons, voice orbs, ~260MB) are included directly in this repo under `brand-assets/`. The `asset-setup` skill copies them into your Remotion project's `public/brand-assets/` directory.

## V2 Design System

The plugin includes the full v2 visual system:

- **Hero mode** — Gradient backgrounds, frosted BrandedCards, white text
- **Content mode** — OFF_WHITE backgrounds, CREAM cards, GRAPHITE text
- **Hybrid mode** — White left / gradient right split layouts
- **Animation timing** — ENTER_DELAY=35, exitOpacity fade, TRANSITION_FRAMES=45
- **12 v2 scene components** + 2 shared components (GradientBackground, BrandedCard)

## Project Structure

```
elevenlabs-academy-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── skills/
│   ├── asset-setup/         # Project bootstrap + asset download
│   ├── remotion-builder/    # Code generation from specs
│   │   ├── rules/           # Brand system, animations, components, compositions
│   │   └── references/      # Component inventory, bundled assets
│   ├── remotion-spec/       # Spec drafting
│   │   ├── rules/           # Remotion API reference files
│   │   └── references/      # Brand guidelines, asset inventory
│   ├── remotion-best-practices/  # Remotion API patterns
│   │   └── rules/           # Animations, timing, transitions, audio, 3D, etc.
│   ├── lesson-builder/      # Lesson content orchestrator
│   ├── lesson-author/       # VO script authoring
│   ├── brand/               # Brand guideline enforcement
│   └── project-context/     # Curriculum and project context
└── README.md
```

## License

MIT
