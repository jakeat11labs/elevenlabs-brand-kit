<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="brand-assets/logos/Logo-white.png">
    <img src="brand-assets/logos/Logo-black.png" alt="ElevenLabs" width="300" />
  </picture>
</p>

<h1 align="center">ElevenLabs Brand Kit</h1>

A Claude Code plugin for producing on-brand ElevenLabs content -- web apps, landing pages, marketing sites, presentations, and Remotion videos. Brand assets, design guidelines, and creative workflows in one package.

<p align="center">
  <a href="#for-claude-code"><img src="https://img.shields.io/badge/Claude_Code-Install_Plugin-6B4FBB?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0id2hpdGUiPjxwYXRoIGQ9Ik04IDNoOHYySDh6TTYgN2gxMnYySDZ6TTQgMTFoMTZ2Mkg0eiIvPjwvc3ZnPg==" alt="Claude Code"></a>
  <a href="#for-claude-desktop--cowork-desktop"><img src="https://img.shields.io/badge/Claude_Desktop-Download_.skill-E8491D?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0id2hpdGUiPjxwYXRoIGQ9Ik0xMiAyTDEyIDE1TTEyIDE1TDcgMTBNMTIgMTVMMTcgMTBNNSAxOEgxOSIgc3Ryb2tlPSJ3aGl0ZSIgc3Ryb2tlLXdpZHRoPSIyIiBmaWxsPSJub25lIiBzdHJva2UtbGluZWNhcD0icm91bmQiLz48L3N2Zz4=" alt="Claude Desktop"></a>
  <a href="#brand-assets"><img src="https://img.shields.io/badge/Brand_Assets-~75MB-1E1916?style=for-the-badge" alt="Brand Assets"></a>
  <a href="#whats-included"><img src="https://img.shields.io/badge/Skills-7_Included-F5F3F1?style=for-the-badge&labelColor=1E1916" alt="Skills"></a>
</p>

## What's Included

| Skill | Command | What It Does |
|-------|---------|-------------|
| **Asset Setup** | `/elevenlabs-brand-kit:asset-setup` | Download brand assets (~75MB), bootstrap a project, configure storage |
| **Brand** | `/elevenlabs-brand-kit:brand` | Enforce ElevenLabs brand guidelines on any content |
| **Branded Web** | `/elevenlabs-brand-kit:branded-web` | Build ElevenLabs-branded web experiences (HTML, CSS, React, Tailwind, shadcn/ui) |
| **Branded PPTX** | `/elevenlabs-brand-kit:eleven-branded-pptx` | Create ElevenLabs-branded PowerPoint presentations from the included 30-slide template |
| **Remotion Spec Builder** | `/elevenlabs-brand-kit:remotion-spec` | Draft scene-by-scene video blueprints with layouts, modes, backgrounds |
| **Remotion Builder** | `/elevenlabs-brand-kit:remotion-builder` | Generate React/TypeScript Remotion compositions from spec files |
| **Remotion Best Practices** | `/elevenlabs-brand-kit:remotion-best-practices` | Remotion API reference -- animations, transitions, audio, video, fonts, 3D |

### When to use which

- **"Build a branded web page"** -> **Branded Web** -- CSS tokens, card variants, font loading, noise overlays, Tailwind/shadcn setup
- **"Make a presentation"** -> **Branded PPTX** -- ElevenLabs-branded decks from the included 30-slide template (requires `/pptx` skill)
- **"Is this on-brand?"** -> **Brand** -- checks content against ElevenLabs brand guidelines
- **"Set up a new project"** -> **Asset Setup** -- downloads brand assets and bootstraps the project
- **"Spec out a video"** -> **Remotion Spec Builder** -- creates the video blueprint (scene layouts, modes, durations)
- **"Implement this spec"** -> **Remotion Builder** -- generates the actual React code from a spec file

## Installation

### For Claude Code

**Via marketplace (recommended):**

```
/plugin marketplace add jakeat11labs/elevenlabs-brand-kit
```

```
/plugin install elevenlabs-brand-kit@elevenlabs-brand-kit
```

**Local development:**

```bash
git clone https://github.com/jakeat11labs/elevenlabs-brand-kit.git
claude --plugin-dir ./elevenlabs-brand-kit
```

### For Claude Desktop / Cowork Desktop

> **Note:** Only the **Branded PPTX** skill is currently packaged for Claude Desktop / Cowork Desktop. The rest of the kit (web, Remotion, asset-setup, brand) is Claude Code-only for now. More standalone skills are on the way.

Individual skills are available as standalone `.skill` files that bundle everything needed -- no plugin setup required.

**How to install (easy way):**

1. Download the `.skill` file from the table below
2. **Double-click it** -- Claude Desktop / Cowork Desktop should open and show this dialog:

<p align="center">
  <img src="docs/images/skill-install-dialog.jpg" alt="Add skill to library dialog" width="700">
</p>

3. Click **Add to library** and you're done.

**If double-click doesn't open the dialog** (manual install):

1. Open Claude Desktop or Cowork Desktop
2. Go to **Customize** in the sidebar (towards the top)
3. Click **Skills** at the very top
4. Click the **+** button under Skills
5. Under **Create Skill**, select **Upload a Skill**
6. Select the downloaded `.skill` file

**Available standalone skills:**

| Skill | Download | Size | What's Included |
|-------|----------|------|----------------|
| **Branded PPTX** | [eleven-branded-pptx.skill](https://github.com/jakeat11labs/elevenlabs-brand-kit/releases/latest/download/eleven-branded-pptx.skill) | ~11MB | 30-slide ElevenLabs template with 54 embedded media files (backgrounds, voice orbs, icons, logos), brand tokens, layout guide |

Each `.skill` file is self-contained -- it bundles the template and all reference materials so the agent can create on-brand presentations without any additional downloads.

### For Any Agent (via skills.sh)

Use the [`skills` CLI](https://skills.sh) to install individual skills into any supported agent -- Claude Code, Codex, Cursor, OpenCode, and [40+ others](https://github.com/vercel-labs/skills#available-agents). No plugin required.

**Install everything (all 7 skills):**

```bash
npx skills add jakeat11labs/elevenlabs-brand-kit
```

**Install a specific skill:**

```bash
# Asset Setup -- bootstrap a project, download brand assets
npx skills add jakeat11labs/elevenlabs-brand-kit --skill asset-setup

# Brand -- enforce ElevenLabs brand guidelines
npx skills add jakeat11labs/elevenlabs-brand-kit --skill elevenlabs-brand

# Branded Web -- build ElevenLabs-branded web experiences
npx skills add jakeat11labs/elevenlabs-brand-kit --skill branded-web

# Branded PPTX -- ElevenLabs-branded PowerPoint presentations
npx skills add jakeat11labs/elevenlabs-brand-kit --skill eleven-branded-pptx

# Remotion Spec -- draft scene-by-scene video blueprints
npx skills add jakeat11labs/elevenlabs-brand-kit --skill remotion-spec-builder

# Remotion Builder -- generate Remotion compositions from specs
npx skills add jakeat11labs/elevenlabs-brand-kit --skill remotion-builder

# Remotion Best Practices -- Remotion API reference
npx skills add jakeat11labs/elevenlabs-brand-kit --skill remotion-best-practices
```

Add `-g` to install globally (available across all projects), or `-a <agent>` to target a specific agent (e.g. `-a claude-code`, `-a codex`, `-a cursor`). See the [`skills` CLI docs](https://skills.sh/docs/cli) for all options.

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

### For Presentations

1. Install the plugin (or download the standalone [`eleven-branded-pptx.skill`](https://github.com/jakeat11labs/elevenlabs-brand-kit/releases/latest/download/eleven-branded-pptx.skill) for Claude Desktop)
2. Run `/elevenlabs-brand-kit:asset-setup` to download brand assets (includes the PowerPoint template)
3. Create branded decks with `/elevenlabs-brand-kit:eleven-branded-pptx` (requires the `/pptx` skill for file tooling)

## Brand Assets

Brand assets (~75MB optimized) are distributed via [GitHub Releases](https://github.com/jakeat11labs/elevenlabs-brand-kit/releases). The `asset-setup` skill downloads them and supports both project-local and central (`~/.elevenlabs-kit/`) storage.

**Included:**
- **52+ background images** -- gradients, agents-themed, Chladni close-ups, shared textures
- **400+ brand icons** -- cream, black, and white variants (130 each) + 10 product icons
- **8 voice orbs** -- metallic spheres in teal, purple, pink, coral, green, gold, hotpink, lavender
- **Logos** -- black + white wordmark, II symbol
- **4 KMR Waldenburg fonts** -- Book, Normal, Halbfett, Fett weights
- **Color tokens** (`color-tokens.json`) + **Brand Guidelines** (`BRAND_GUIDELINES.md`)
- **Presentation template** -- 30-slide branded PowerPoint with 80 layout masters and 54 embedded media
- **Visual catalog** (`index.html`) -- browse every asset with one-click copy buttons

## V2 Design System

The kit includes the full ElevenLabs V2 visual system, applicable to web, video, and presentations:

- **Hero mode** -- Gradient backgrounds, frosted BrandedCards, white text
- **Content mode** -- OFF_WHITE backgrounds, CREAM cards, GRAPHITE text
- **Hybrid mode** -- White left / gradient right split layouts
- **8 BrandedCard variants** -- darkflat, darkglass, glass, baseline, outline, pill, acrylic, gradientborder
- **Monochrome palette** -- Graphite, Off-White, Cream, Light-Gray, Mid-Gray; color comes from imagery

## Project Structure

```
elevenlabs-brand-kit/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest
│   └── marketplace.json         # Marketplace catalog
├── brand-assets/                # ~75MB optimized brand assets
│   ├── backgrounds/             # Gradients, agents, Chladni, textures
│   ├── icons/                   # 130 icons x 3 variants + product icons
│   ├── voice-orbs/              # 8 metallic orb images
│   ├── logos/                   # Wordmark + II symbol
│   ├── fonts/                   # KMR Waldenburg (4 weights)
│   ├── elevenlabs-presentation-template.pptx  # 30-slide branded template
│   ├── BRAND_GUIDELINES.md      # Official 2025 brand guidelines
│   ├── CLAUDE.md                # Asset inventory + usage guide
│   ├── color-tokens.json        # Color definitions
│   └── index.html               # Visual asset catalog
├── skills/
│   ├── asset-setup/             # Project bootstrap + asset download
│   ├── brand/                   # Brand guideline enforcement
│   ├── eleven-branded-pptx/            # ElevenLabs-branded presentations
│   │   └── references/          # Template layout guide
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
├── standalone/
│   └── eleven-branded-pptx/            # Standalone skill source (packaged into .skill)
│       ├── SKILL.md
│       ├── references/
│       └── assets/              # Bundled template PPTX
├── eleven-branded-pptx.skill           # Downloadable standalone skill (zip)
└── README.md
```

## License

For ElevenLabs internal use only. This plugin is for authorized use by ElevenLabs employees. Brand assets, guidelines, fonts, and curriculum content are proprietary to ElevenLabs. No unauthorized use, reproduction, or distribution is permitted.
