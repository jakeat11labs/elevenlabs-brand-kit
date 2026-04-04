---
name: setup
description: "Bootstrap a new ElevenLabs Academy Remotion project from scratch — installs Remotion, downloads brand assets, sets up fonts, and verifies everything works. Use when someone says 'setup the project', 'bootstrap', 'install remotion', 'get started', 'new project setup', 'download assets', or when starting from zero. Also use when brand assets are missing or the project needs to be re-initialized."
---

# ElevenLabs Academy — Project Setup

This skill bootstraps a complete ElevenLabs Academy Remotion project from scratch, or repairs an existing one that's missing dependencies or assets.

## What It Does

1. **Checks prerequisites** — Node.js, npm
2. **Creates or validates the Remotion project** — installs Remotion 4.0+ with React 19
3. **Downloads brand assets** — backgrounds, icons, voice orbs, diagrams from GitHub
4. **Sets up fonts** — KMR Waldenburg OTF files
5. **Verifies the setup** — runs `npx tsc --noEmit` to confirm everything compiles

## Full Bootstrap (New Project)

Run these steps in order:

### Step 1: Create the Remotion project

```bash
# If starting fresh, create a new Remotion project
npx create-video@latest elevenlabs-academy --template blank
cd elevenlabs-academy

# Install required packages
npm install remotion @remotion/cli @remotion/transitions @remotion/fonts @remotion/light-leaks zod
```

### Step 2: Download brand assets

```bash
# Clone the asset repo
git clone https://github.com/jakerains/elevenlabs-academy-assets.git /tmp/el-academy-assets

# Create the brand-assets directory
mkdir -p public/brand-assets

# Copy assets into the project
cp -r /tmp/el-academy-assets/brand-assets/* public/brand-assets/

# Clean up
rm -rf /tmp/el-academy-assets

echo "Brand assets installed."
```

### Step 3: Verify fonts

```bash
# Check that KMR Waldenburg fonts are present
ls public/fonts/KMR-Waldenburg-*.otf 2>/dev/null && echo "Fonts OK" || echo "WARNING: KMR Waldenburg fonts missing from public/fonts/"
```

### Step 4: Verify brand assets

```bash
# Quick check — all 5 asset categories should have files
echo "Backgrounds: $(ls public/brand-assets/backgrounds/gradient/ 2>/dev/null | wc -l) gradient files"
echo "Icons: $(ls public/brand-assets/icons/cream/ 2>/dev/null | wc -l) cream icons"
echo "Voice Orbs: $(ls public/brand-assets/voice-orbs/ 2>/dev/null | wc -l) orb files"
echo "Diagrams: $(ls public/brand-assets/diagrams/ 2>/dev/null | wc -l) diagram files"
echo "Guidelines: $(ls public/brand-assets/BRAND_GUIDELINES.md 2>/dev/null && echo 'present' || echo 'missing')"
```

### Step 5: Verify TypeScript

```bash
npx tsc --noEmit
```

## Repair Mode (Existing Project)

If the project exists but is missing assets or dependencies:

### Missing brand assets only
```bash
if [ ! -d "public/brand-assets/backgrounds" ]; then
  git clone https://github.com/jakerains/elevenlabs-academy-assets.git /tmp/el-academy-assets
  mkdir -p public/brand-assets
  cp -r /tmp/el-academy-assets/brand-assets/* public/brand-assets/
  rm -rf /tmp/el-academy-assets
  echo "Brand assets restored."
else
  echo "Brand assets already present."
fi
```

### Missing npm packages
```bash
npm install remotion @remotion/cli @remotion/transitions @remotion/fonts @remotion/light-leaks zod
```

## What Gets Installed

### Brand Assets (`public/brand-assets/`)
| Category | Count | Location |
|----------|-------|----------|
| Gradient backgrounds | 18 | `backgrounds/gradient/` |
| Agent backgrounds | 9 | `backgrounds/agents/` |
| Chladni close-up backgrounds | 15 | `backgrounds/chladni-closeup/` |
| General backgrounds | 7 | `backgrounds/general/` |
| Creative backgrounds | 1 | `backgrounds/creative/` |
| Shared textures | 2 | `backgrounds/` |
| Cream icons | 130 | `icons/cream/` |
| Black icons | 130 | `icons/black/` |
| White icons | 130 | `icons/white/` |
| Product icons | 10 | `icons/product/` |
| Voice orbs | 8 | `voice-orbs/` |
| Diagrams | 1 | `diagrams/` |
| UI highlights | 1 | `ui-highlights/` |
| Color tokens | 1 | `color-tokens.json` |
| Brand guidelines | 1 | `BRAND_GUIDELINES.md` |
| Asset inventory | 1 | `CLAUDE.md` |
| Visual catalogs | 4 | `*.html` files |

### Fonts (`public/fonts/`)
- KMR Waldenburg Buch (300)
- KMR Waldenburg Normal (400)
- KMR Waldenburg Halbfett (600)
- KMR Waldenburg Fett (700)

### NPM Packages
- `remotion` — Core framework
- `@remotion/cli` — CLI tools
- `@remotion/transitions` — TransitionSeries, fade, slide, etc.
- `@remotion/fonts` — Font loading
- `@remotion/light-leaks` — Light leak overlay effects
- `zod` — Schema validation

## Asset Repository

**GitHub:** `https://github.com/jakerains/elevenlabs-academy-assets`

Contains brand asset files only (images, icons, tokens, guidelines). Does NOT contain Remotion source code — that lives in the project repo.

## After Setup

Once the project is bootstrapped, use the other plugin skills:

- `/elevenlabs-academy:remotion-spec` — Draft a spec for a new lesson video
- `/elevenlabs-academy:remotion-builder` — Build the composition from a spec
- `/elevenlabs-academy:brand` — Enforce brand guidelines on any content
- `/elevenlabs-academy:remotion-best-practices` — Remotion API reference and patterns
