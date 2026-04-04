---
name: setup
description: "Bootstrap a new ElevenLabs Academy Remotion project — installs Remotion, downloads brand assets from this plugin's repo, sets up fonts, and verifies everything works. Use when someone says 'setup the project', 'bootstrap', 'install remotion', 'get started', 'new project setup', 'download assets', or when starting from zero. Also use when brand assets are missing or the project needs to be re-initialized."
---

# ElevenLabs Academy — Project Setup

This skill bootstraps a complete ElevenLabs Academy Remotion project, or repairs one that's missing dependencies or assets.

## What It Does

1. Checks prerequisites (Node.js, npm)
2. Creates or validates the Remotion project
3. Downloads brand assets from the plugin repo's `brand-assets/` directory
4. Sets up fonts
5. Verifies with `npx tsc --noEmit`

## Full Bootstrap (New Project)

### Step 1: Create the Remotion project

```bash
npx create-video@latest elevenlabs-academy --template blank
cd elevenlabs-academy

npm install remotion @remotion/cli @remotion/transitions @remotion/fonts @remotion/light-leaks zod
```

### Step 2: Download brand assets

The brand assets live in this plugin's repo at `brand-assets/`. Copy them into the project:

```bash
# If the plugin is installed locally, copy from the plugin directory
PLUGIN_DIR=$(find ~/.claude/plugins -name "elevenlabs-academy" -type d 2>/dev/null | head -1)

if [ -n "$PLUGIN_DIR" ] && [ -d "$PLUGIN_DIR/brand-assets" ]; then
  mkdir -p public/brand-assets
  cp -r "$PLUGIN_DIR/brand-assets/"* public/brand-assets/
  echo "Brand assets copied from installed plugin."
else
  # Fall back to cloning from GitHub
  git clone --depth 1 https://github.com/jakerains/elevenlabs-academy-plugin.git /tmp/el-academy
  mkdir -p public/brand-assets
  cp -r /tmp/el-academy/brand-assets/* public/brand-assets/
  rm -rf /tmp/el-academy
  echo "Brand assets downloaded from GitHub."
fi
```

### Step 3: Verify fonts

```bash
ls public/fonts/KMR-Waldenburg-*.otf 2>/dev/null && echo "Fonts OK" || echo "WARNING: Fonts missing. Copy from plugin: skills/remotion-spec/assets/fonts/"
```

If fonts are missing, copy them from the plugin:
```bash
mkdir -p public/fonts
cp "$PLUGIN_DIR/skills/remotion-spec/assets/fonts/"*.otf public/fonts/
```

### Step 4: Verify

```bash
echo "Backgrounds: $(ls public/brand-assets/backgrounds/gradient/ 2>/dev/null | wc -l) gradient files"
echo "Icons: $(ls public/brand-assets/icons/cream/ 2>/dev/null | wc -l) cream icons"
echo "Voice Orbs: $(ls public/brand-assets/voice-orbs/ 2>/dev/null | wc -l) orb files"
npx tsc --noEmit
```

## Repair Mode (Existing Project)

### Missing brand assets only
```bash
PLUGIN_DIR=$(find ~/.claude/plugins -name "elevenlabs-academy" -type d 2>/dev/null | head -1)
if [ -d "$PLUGIN_DIR/brand-assets" ]; then
  cp -r "$PLUGIN_DIR/brand-assets/"* public/brand-assets/
else
  git clone --depth 1 https://github.com/jakerains/elevenlabs-academy-plugin.git /tmp/el-academy
  cp -r /tmp/el-academy/brand-assets/* public/brand-assets/
  rm -rf /tmp/el-academy
fi
```

### Missing npm packages
```bash
npm install remotion @remotion/cli @remotion/transitions @remotion/fonts @remotion/light-leaks zod
```

## What Gets Installed

### Brand Assets (`public/brand-assets/`)
- 52 background images (gradient, agents, chladni, general, creative)
- 400 brand icons (cream, black, white, product)
- 8 voice orbs
- Diagrams, UI highlights, color tokens, brand guidelines
- HTML visual catalogs

### Fonts (`public/fonts/`)
- KMR Waldenburg: Buch (300), Normal (400), Halbfett (600), Fett (700)

### NPM Packages
- `remotion`, `@remotion/cli`, `@remotion/transitions`, `@remotion/fonts`, `@remotion/light-leaks`, `zod`

## After Setup

Use the other plugin skills:
- `/elevenlabs-academy:remotion-spec` — Draft a video spec
- `/elevenlabs-academy:remotion-builder` — Build the composition from a spec
- `/elevenlabs-academy:brand` — Enforce brand guidelines
- `/elevenlabs-academy:remotion-best-practices` — Remotion API reference
