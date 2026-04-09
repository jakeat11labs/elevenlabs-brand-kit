<p align="center">
  <img src="brand-assets/logos/Logo-black.png" alt="ElevenLabs" width="300" />
</p>

# ElevenLabs Remotion Kit

A Claude Code plugin for producing ElevenLabs branded videos using Remotion — spec drafting, code generation, brand enforcement, and best practices.

## What's Included

| Skill | Command | What It Does |
|-------|---------|-------------|
| **Asset Setup** | `/elevenlabs-remotion-kit:asset-setup` | Download brand assets, bootstrap a Remotion project, configure central or project-local storage |
| **Remotion Spec Builder** | `/elevenlabs-remotion-kit:remotion-spec` | Draft scene-by-scene video blueprints with layouts, modes, backgrounds, and content |
| **Remotion Builder** | `/elevenlabs-remotion-kit:remotion-builder` | Generate React/TypeScript Remotion compositions from spec files |
| **Remotion Best Practices** | `/elevenlabs-remotion-kit:remotion-best-practices` | Remotion API reference — animations, transitions, audio, video, fonts, 3D |
| **Brand** | `/elevenlabs-remotion-kit:brand` | Enforce ElevenLabs brand guidelines on any content |

### When to use which

- **"Spec out a video"** → **Remotion Spec Builder** — creates the video blueprint (scene layouts, modes, durations)
- **"Implement this spec"** → **Remotion Builder** — generates the actual React code from a spec file
- **"Set up a new project"** → **Asset Setup** — downloads brand assets and bootstraps the Remotion project
- **"Is this on-brand?"** → **Brand** — checks content against ElevenLabs brand guidelines

## Installation

### From GitHub (recommended)

```bash
claude /plugin install https://github.com/jakerains/elevenlabs-remotion-kit
```

### Local development

```bash
git clone https://github.com/jakerains/elevenlabs-remotion-kit.git
claude --plugin-dir ./elevenlabs-remotion-kit
```

## Quick Start

1. Install the plugin
2. Run `/elevenlabs-remotion-kit:asset-setup` to bootstrap your Remotion project with brand assets
3. Draft a spec with `/elevenlabs-remotion-kit:remotion-spec`
4. Build it with `/elevenlabs-remotion-kit:remotion-builder`

## Brand Assets

Brand assets (backgrounds, icons, voice orbs, fonts, logos — ~278MB optimized) are distributed via [GitHub Releases](https://github.com/jakerains/elevenlabs-remotion-kit/releases). The `asset-setup` skill downloads them and supports both project-local and central (`~/.elevenlabs-academy/`) storage.

## V2 Design System

The plugin includes the full v2 visual system:

- **Hero mode** — Gradient backgrounds, frosted BrandedCards, white text
- **Content mode** — OFF_WHITE backgrounds, CREAM cards, GRAPHITE text
- **Hybrid mode** — White left / gradient right split layouts
- **Animation timing** — ENTER_DELAY=35, exitOpacity fade, TRANSITION_FRAMES=45
- **12 v2 scene components** + 2 shared components (GradientBackground, BrandedCard)

## Project Structure

```
elevenlabs-remotion-kit/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── skills/
│   ├── asset-setup/             # Project bootstrap + asset download
│   ├── brand/                   # Brand guideline enforcement
│   ├── remotion-best-practices/ # Remotion API patterns
│   │   └── rules/               # Animations, timing, transitions, audio, 3D, etc.
│   ├── remotion-builder/        # Code generation from specs
│   │   ├── rules/               # Brand system, animations, components, compositions
│   │   └── references/          # Component inventory, bundled assets
│   └── remotion-spec/           # Spec drafting
│       ├── assets/              # Spec templates and examples
│       ├── rules/               # Remotion API reference files
│       └── references/          # Brand guidelines, asset inventory
└── README.md
```

## License

For ElevenLabs internal use only. This plugin is for authorized use by ElevenLabs employees. Brand assets, guidelines, fonts, and curriculum content are proprietary to ElevenLabs. No unauthorized use, reproduction, or distribution is permitted.
