---
name: asset-setup
description: "Download ElevenLabs brand assets (backgrounds, icons, fonts, voice orbs, logos) and bootstrap a project. Use when someone says 'setup assets', 'download brand assets', 'get the assets', 'bootstrap', 'install assets', 'new project setup', 'asset setup', or when brand assets are missing. Also handles updating assets to a newer version."
---

# ElevenLabs Brand Kit — Asset Setup

This skill downloads the official ElevenLabs brand asset pack and sets up a project to use them. It supports two storage modes: **project-local** (assets live inside the project) or **central** (single shared copy across all projects).

## Overview

Brand assets are distributed as a versioned zip from GitHub Releases. The zip contains:
- `brand-assets/` — backgrounds, icons, voice orbs, logos, diagrams, screenshots, images, color tokens, brand guidelines, visual catalogs, and fonts

**Note on fonts:** KMR Waldenburg OTFs are bundled inside `brand-assets/fonts/`. During installation they are copied to both `public/brand-assets/fonts/` (for the catalog) and `public/fonts/` (for local web and video tooling). Both locations are populated automatically.

**Current version:** `v3.1.1`
**Download URL:** `https://github.com/jakeat11labs/elevenlabs-brand-kit/releases/download/v3.1.1/brand-assets-v3.1.1.zip`

---

## Step 0: Pre-Flight Check

**Run these checks silently before asking the user anything.** Both can run in parallel.

**Check existing config + project state:**
```bash
CONFIG_FILE="$HOME/.elevenlabs-kit/config.json"
if [ -f "$CONFIG_FILE" ]; then
  INSTALLED_VERSION=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE')).get('assetVersion','none'))" 2>/dev/null)
  ASSET_LOCATION=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE')).get('assetLocation','none'))" 2>/dev/null)
  CENTRAL_PATH=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE')).get('centralPath',''))" 2>/dev/null)
else
  INSTALLED_VERSION="none"
  ASSET_LOCATION="none"
fi

# Check if this project already has brand assets linked/installed
if [ -L "public/brand-assets" ]; then
  PROJECT_STATUS="linked"
elif [ -d "public/brand-assets" ]; then
  PROJECT_STATUS="local"
else
  PROJECT_STATUS="none"
fi
```

**Check GitHub for the latest release:**
```bash
LATEST_INFO=$(curl -s https://api.github.com/repos/jakeat11labs/elevenlabs-brand-kit/releases/latest)
LATEST_VERSION=$(echo "$LATEST_INFO" | python3 -c "import json,sys; print(json.load(sys.stdin)['tag_name'])" 2>/dev/null)
ASSET_URL=$(echo "$LATEST_INFO" | python3 -c "import json,sys; assets=json.load(sys.stdin)['assets']; print(next(a['browser_download_url'] for a in assets if a['name'].endswith('.zip')))" 2>/dev/null)
```

Based on these results, proceed to **Step 1**.

---

## Step 1: Route & Setup

Based on the pre-flight results from Step 0, follow **exactly one path:**

### Path A — Existing central install, assets current

**Condition:** Config exists, `assetLocation` is `"central"`, version matches latest, and central assets directory exists.

This is the fast path — assets are already downloaded centrally. Just link this project and set up the right tooling.

**Tell the user:**
> Brand assets already set up centrally (v$INSTALLED_VERSION). I'll link this project to your shared assets.

**Ask project type only** — no storage question needed since they already chose central:

```
AskUserQuestion({
  questions: [{
    question: "What kind of project are you linking?",
    header: "Project type",
    options: [
      { label: "Just the brand assets", description: "Link brand-assets only — no npm packages, no project-specific guidance" },
      { label: "HTML / web project", description: "Link brand assets and surface web-specific guidance (CSS tokens, React, Tailwind, shadcn)" },
      { label: "HyperFrames video project", description: "Link brand assets and bootstrap HyperFrames dependencies" },
      { label: "Presentations", description: "Link brand assets for creating branded PowerPoint decks" }
    ],
    multiSelect: false
  }]
})
```

Store as `PROJECT_TYPE` (`assets`, `html`, `hyperframes`, or `pptx`). Then → **Step 4** (link) → **Step 5** (verify) → project-specific bootstrap if needed.

### Path B — Existing install, version outdated

**Condition:** Config exists, version doesn't match latest.

Ask if they want to update:

```
AskUserQuestion({
  questions: [{
    question: "Update available: your assets are at v$INSTALLED_VERSION — v$LATEST_VERSION is available. Want me to update now?",
    header: "Update",
    options: [
      { label: "Yes, update now", description: "Download and install the latest brand assets" },
      { label: "No, skip", description: "Keep the current version and just link this project" }
    ],
    multiSelect: false
  }]
})
```

Then ask project type (same UI as Path A). If update → **Step 3** (download, use existing `ASSET_LOCATION`) → **Step 4** (link if central) → **Step 5** (verify). If skip → **Step 4** (link if central) → **Step 5** (verify).

### Path C — First-time setup (no config)

**Condition:** No config file exists (`INSTALLED_VERSION="none"`).

Full setup flow — ask both questions in sequence:

**C1: Project Type**

```
AskUserQuestion({
  questions: [{
    question: "What kind of project are you setting up?",
    header: "Project type",
    options: [
      { label: "Just the brand assets", description: "Install brand-assets only — no npm packages, no project-specific guidance. Good for reference use or pulling individual files. Includes the visual catalog (index.html)" },
      { label: "HTML / web project", description: "Install brand assets and surface web-specific guidance (CSS tokens, React, Tailwind, shadcn)" },
      { label: "HyperFrames video project", description: "Install brand assets and bootstrap HyperFrames dependencies" },
      { label: "Presentations", description: "Install brand assets for creating branded PowerPoint decks" }
    ],
    multiSelect: false
  }]
})
```

Store as `PROJECT_TYPE` (`assets`, `html`, `hyperframes`, or `pptx`). If the user picks "Other" (the auto-provided escape hatch), treat it as `assets`.

**C2: Storage Preference**

```
AskUserQuestion({
  questions: [{
    question: "Where should I store the brand assets?",
    header: "Storage",
    options: [
      { label: "Project-local", description: "Downloads assets into this project's public/ directory. Each project gets its own copy. Best for single-project workflows." },
      { label: "Central (shared)", description: "Downloads once to ~/.elevenlabs-kit/ and symlinks into each project. Saves disk space — best for multiple ElevenLabs projects." }
    ],
    multiSelect: false
  }]
})
```

Store as `STORAGE_PREF`. Then → **Step 3** (download) → **Step 4** (link if central) → **Step 5** (verify) → project-specific bootstrap.

### Path D — Config exists, assets missing or broken

**Condition:** Config exists but assets can't be found at the configured location.

Tell the user assets are missing. Ask project type (same UI as Path A), then re-download using the stored `ASSET_LOCATION` → **Step 3** → **Step 4** (link if central) → **Step 5** (verify).

### Path E — Project already linked/installed, assets current

**Condition:** `PROJECT_STATUS` is `"linked"` or `"local"` AND version matches latest.

Assets are already in this project and up to date. Tell the user:
> Brand assets already installed in this project (v$INSTALLED_VERSION) — everything is current.

Ask project type for post-setup guidance only — no download or linking needed. Skip to project-specific bootstrap if applicable, then show the **After Setup** guidance for their project type.

---

## Step 3: Download and Extract Assets

### Download the zip

Use `$ASSET_URL` and `$LATEST_VERSION` from Step 0. If the pre-flight GitHub check failed (no network), fall back to the hardcoded URL below.

```bash
# $ASSET_URL and $LATEST_VERSION set in Step 0
# Fallback if network check failed:
ASSET_URL="${ASSET_URL:-https://github.com/jakeat11labs/elevenlabs-brand-kit/releases/download/v3.1.1/brand-assets-v3.1.1.zip}"
LATEST_VERSION="${LATEST_VERSION:-3.1.1}"
TEMP_ZIP="/tmp/elevenlabs-brand-assets-latest.zip"

curl -L -o "$TEMP_ZIP" "$ASSET_URL"
echo "Downloaded $(du -h "$TEMP_ZIP" | cut -f1) — version $LATEST_VERSION"
```

### Option 1: Project-Local Storage

```bash
# Extract directly into the project
mkdir -p public/brand-assets public/fonts
unzip -o "$TEMP_ZIP" -d /tmp/elevenlabs-assets-extract

# Copy all brand assets (fonts are inside brand-assets/fonts/)
cp -r /tmp/elevenlabs-assets-extract/brand-assets/* public/brand-assets/

# Also copy fonts to public/fonts/ for local web and video tooling
cp -r /tmp/elevenlabs-assets-extract/brand-assets/fonts/* public/fonts/

# Clean up
rm -rf /tmp/elevenlabs-assets-extract "$TEMP_ZIP"

echo "Brand assets installed to public/brand-assets/"
echo "Fonts installed to public/fonts/ (also at public/brand-assets/fonts/)"
```

Save config:
```bash
mkdir -p "$HOME/.elevenlabs-kit"
cat > "$HOME/.elevenlabs-kit/config.json" << CONFIGEOF
{
  "assetLocation": "project-local",
  "assetVersion": "${LATEST_VERSION#v}",
  "lastUpdated": "$(date +%Y-%m-%d)"
}
CONFIGEOF
```

### Option 2: Central (Shared) Storage

```bash
CENTRAL_DIR="$HOME/.elevenlabs-kit"
mkdir -p "$CENTRAL_DIR"

# Extract to central location
unzip -o "$TEMP_ZIP" -d "$CENTRAL_DIR"

# Clean up temp
rm -f "$TEMP_ZIP"

echo "Brand assets installed to $CENTRAL_DIR/brand-assets/"
echo "Fonts installed to $CENTRAL_DIR/fonts/"
```

Then create symlinks in the current project:
```bash
# Create public/ if it doesn't exist
mkdir -p public

# Remove existing directories/symlinks if present
rm -rf public/brand-assets public/fonts

# Symlink brand-assets to central store
ln -s "$HOME/.elevenlabs-kit/brand-assets" public/brand-assets

# Fonts are inside brand-assets/fonts/ — symlink that subdirectory to public/fonts/
ln -s "$HOME/.elevenlabs-kit/brand-assets/fonts" public/fonts

echo "Symlinked public/brand-assets → $CENTRAL_DIR/brand-assets/"
echo "Symlinked public/fonts → $CENTRAL_DIR/brand-assets/fonts/"
```

Save config:
```bash
cat > "$HOME/.elevenlabs-kit/config.json" << CONFIGEOF
{
  "assetLocation": "central",
  "centralPath": "$HOME/.elevenlabs-kit",
  "assetVersion": "${LATEST_VERSION#v}",
  "lastUpdated": "$(date +%Y-%m-%d)"
}
CONFIGEOF
```

### Write brand context files

Write `CLAUDE.md` and `AGENTS.md` to `~/.elevenlabs-kit/` so any AI agent that accesses the central asset store has full brand context, color tokens, typography rules, icon selection guidance, and a compliance checklist.

Read the template files from this skill's `templates/` directory:
- `templates/central-CLAUDE.md` → Write to `$HOME/.elevenlabs-kit/CLAUDE.md`
- `templates/central-AGENTS.md` → Write to `$HOME/.elevenlabs-kit/AGENTS.md`

These files ensure any agent — Claude, Copilot, Cursor, or otherwise — that peeks into the central store knows exactly how to stay on-brand without needing the plugin installed.

---

## Step 4: Link Central Assets to a New Project

When using central storage and setting up a **new** project (assets already downloaded), just create the symlinks:

```bash
CONFIG_FILE="$HOME/.elevenlabs-kit/config.json"

# Verify central assets exist
CENTRAL_DIR=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE'))['centralPath'])" 2>/dev/null)

if [ -d "$CENTRAL_DIR/brand-assets" ]; then
  mkdir -p public
  rm -rf public/brand-assets public/fonts
  ln -s "$CENTRAL_DIR/brand-assets" public/brand-assets
  ln -s "$CENTRAL_DIR/brand-assets/fonts" public/fonts
  echo "Linked to central assets at $CENTRAL_DIR"
else
  echo "Central assets not found — run full setup first"
fi
```

---

## Step 5: Verify Installation

```bash
echo "=== Asset Verification ==="
echo "Backgrounds: $(find public/brand-assets/backgrounds -type f 2>/dev/null | wc -l | tr -d ' ') files"
echo "  - Gradient: $(ls public/brand-assets/backgrounds/gradient/ 2>/dev/null | wc -l | tr -d ' ')"
echo "  - Agents: $(ls public/brand-assets/backgrounds/agents/ 2>/dev/null | wc -l | tr -d ' ')"
echo "  - Chladni: $(ls public/brand-assets/backgrounds/chladni-closeup/ 2>/dev/null | wc -l | tr -d ' ')"
echo "Icons: $(find public/brand-assets/icons -name '*.png' -type f 2>/dev/null | wc -l | tr -d ' ') PNG files"
echo "Voice Orbs: $(ls public/brand-assets/voice-orbs/*.jpg 2>/dev/null | wc -l | tr -d ' ')"
echo "Logos: $(find public/brand-assets/logos -type f 2>/dev/null | wc -l | tr -d ' ')"
echo "Fonts: $(ls public/fonts/*.otf 2>/dev/null | wc -l | tr -d ' ') OTF files"
echo ""

# Check if symlinked or direct
if [ -L "public/brand-assets" ]; then
  echo "Storage: CENTRAL (symlinked to $(readlink public/brand-assets))"
else
  echo "Storage: PROJECT-LOCAL"
fi

echo ""
echo "Config: $(cat "$HOME/.elevenlabs-kit/config.json" 2>/dev/null || echo 'none')"
```

After verification, tell the user:

> **All done!** Open `public/brand-assets/index.html` in your browser to browse the full asset and component catalog — every background, icon, voice orb, scene, layout, and color token in one place, each with a one-click copy button that outputs a ready-to-paste agent prompt.

### Brand compliance check

After setup completes, invoke `/elevenlabs-brand-kit:brand` to confirm the installed assets are being used correctly and the project is configured for on-brand output. This loads the full brand guidelines into context so all subsequent work in this session adheres to ElevenLabs visual standards.

---

## Repair / Update Mode

### Update assets to a new version

Run asset-setup again — the pre-flight check (Step 0) automatically detects the outdated version and routes to Path B with an update prompt. Central storage only needs one update — all linked projects get it automatically.

### Re-link a project (central mode)

If a project is missing its symlinks but assets exist centrally, run asset-setup — the pre-flight check detects the central install (Path A) and links automatically.

### Switch storage mode

Delete existing assets/symlinks and re-run asset-setup. The pre-flight check will see no assets in the project and route to the appropriate path. If you want to switch from central to project-local (or vice versa), delete `~/.elevenlabs-kit/config.json` first to trigger the full setup flow (Path C) where you can pick a different storage option.

---

## HyperFrames Project Bootstrap

**Only run this section if `PROJECT_TYPE` is `hyperframes` (set in Step 1).**

Asset setup prepares dependencies and scripts. The actual composition scaffold belongs to `/elevenlabs-brand-kit:hyperframes-builder`, which reads the spec and creates the right `index.html`, `meta.json`, `assets/brand-kit.{css,js}`, VO manifest, and timing scripts.

### Install HyperFrames

```bash
# Ensure package.json exists
[ ! -f "package.json" ] && npm init -y

# Install the HyperFrames CLI/runtime locally
npm install -D hyperframes
```

### Wire up scripts

```bash
npm pkg get scripts.doctor 2>/dev/null | grep -qv undefined || npm pkg set scripts.doctor="hyperframes doctor"
npm pkg get scripts.lint 2>/dev/null | grep -qv undefined || npm pkg set scripts.lint="hyperframes lint"
npm pkg get scripts.preview 2>/dev/null | grep -qv undefined || npm pkg set scripts.preview="hyperframes preview --port 3002"
npm pkg get scripts.render 2>/dev/null | grep -qv undefined || npm pkg set scripts.render="hyperframes render"
```

### Smoke test

```bash
# Verify the package is installed
node -e "require.resolve('hyperframes/package.json')" && echo "✓ HyperFrames package OK" || echo "✗ HyperFrames missing — run: npm install -D hyperframes"

# Check the local environment
npx hyperframes doctor || true
```

**After bootstrap, tell the user:**
> HyperFrames is ready. Next: draft the spec with `/elevenlabs-brand-kit:hyperframes-spec`, then build it with `/elevenlabs-brand-kit:hyperframes-builder`. Preview with `npm run preview` once the composition exists.

---

## What Gets Installed

### Brand Assets (`brand-assets/`)
- **52+ background images** — gradient, agents, chladni close-up, general, creative, shared textures
- **400+ brand icons** — cream, black, white variants (130 each) + 10 product icons
- **8 voice orbs** — metallic spheres in teal, purple, pink, coral, green, gold, hotpink, lavender
- **Logos** — black + white, PNG + SVG
- **Covers** — Chladni pattern cover images
- **Images** — general brand imagery
- **Screenshots** — reference screenshots
- **Diagrams & UI highlights**
- **Color tokens** (`color-tokens.json`) + **Brand Guidelines** (`BRAND_GUIDELINES.md`)
- **Unified visual catalog** (`brand-assets/index.html`) — browse every asset, scene, component, and layout in one place with one-click copy buttons

### Fonts (`fonts/`)
- KMR Waldenburg Buch (weight 300)
- KMR Waldenburg Normal (weight 400)
- KMR Waldenburg Halbfett (weight 600)
- KMR Waldenburg Fett (weight 700)

---

## Config File Reference

Location: `~/.elevenlabs-kit/config.json`

```json
{
  "assetLocation": "central | project-local",
  "centralPath": "/Users/you/.elevenlabs-kit",
  "assetVersion": "2.1.0",
  "lastUpdated": "2026-04-05"
}
```

This file is read by all ElevenLabs Brand Kit skills to locate brand assets. When `assetLocation` is `"central"`, skills resolve asset paths through `centralPath` instead of expecting them in the project directory.

---

## After Setup

**First: open the catalog.** `public/brand-assets/index.html` in any browser. Every background, icon, voice orb, scene component, layout, and color token — all with one-click copy buttons that output ready-to-paste agent prompts. Use this to find the right assets before building anything.

**Then, based on your project type:**

**If assets-only (`PROJECT_TYPE=assets`):**
> The brand assets are installed at `public/brand-assets/` (images, icons, voice orbs, logos) and `public/fonts/` (KMR Waldenburg). Open `public/brand-assets/index.html` to browse the visual catalog. You can reference them directly or pull individual files as needed.
> Use `/elevenlabs-brand-kit:brand` to check brand compliance on anything you build.

**If HTML/web (`PROJECT_TYPE=html`):**
> Reference `public/brand-assets/` for images and `public/fonts/` for KMR Waldenburg. Open `public/brand-assets/index.html` to browse the visual catalog.
> Use `/elevenlabs-brand-kit:branded-web` for guidance on implementing brand patterns in CSS/React/Tailwind.
> Use `/elevenlabs-brand-kit:brand` to check brand compliance on anything you build.

**If HyperFrames (`PROJECT_TYPE=hyperframes`):**
> You're ready to build. The typical flow:
> 1. `/elevenlabs-brand-kit:hyperframes-spec` — plan your scenes (layout, mode, content, duration)
> 2. `/elevenlabs-brand-kit:hyperframes-builder` — generate the HTML+GSAP composition from the spec
> 3. `/elevenlabs-brand-kit:hyperframes-cli` — load if you need lint, preview, render, transcription, or TTS command help

**If presentations (`PROJECT_TYPE=pptx`):**
> You're ready to create branded decks. The typical flow:
> 1. `/elevenlabs-brand-kit:eleven-branded-pptx` — create ElevenLabs-branded PowerPoint presentations from the included 30-slide template
> 2. `/elevenlabs-brand-kit:brand` — check brand compliance on your content
> Requires the `/pptx` skill for file tooling.
