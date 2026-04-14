---
name: asset-setup
description: "Download ElevenLabs brand assets (backgrounds, icons, fonts, voice orbs, logos) and bootstrap a project. Use when someone says 'setup assets', 'download brand assets', 'get the assets', 'bootstrap', 'install assets', 'new project setup', 'asset setup', or when brand assets are missing. Also handles updating assets to a newer version."
---

# ElevenLabs Brand Kit — Asset Setup

This skill downloads the official ElevenLabs brand asset pack and sets up a project to use them. It supports two storage modes: **project-local** (assets live inside the project) or **central** (single shared copy across all projects).

## Overview

Brand assets are distributed as a versioned zip from GitHub Releases. The zip contains:
- `brand-assets/` — backgrounds, icons, voice orbs, logos, diagrams, screenshots, images, color tokens, brand guidelines, visual catalogs, and fonts

**Note on fonts:** KMR Waldenburg OTFs are bundled inside `brand-assets/fonts/`. During installation they are copied to both `public/brand-assets/fonts/` (for the catalog) and `public/fonts/` (for web font loading / Remotion staticFile()). Both locations are populated automatically.

**Current version:** `v3.1.0`
**Download URL:** `https://github.com/jakerains/elevenlabs-brand-kit/releases/download/v3.1.0/brand-assets-v3.1.zip`

---

## Step 0: Identify Project Type

**Ask this first — before doing anything else.** Use the `AskUserQuestion` tool for a clean selection UI:

```
AskUserQuestion({
  questions: [{
    question: "What kind of project are you setting up?",
    header: "Project type",
    options: [
      { label: "Just the brand assets", description: "Install brand-assets only — no npm packages, no project-specific guidance. Good for reference use or pulling individual files. Includes the visual catalog (index.html)" },
      { label: "HTML / web project", description: "Install brand assets and surface web-specific guidance (CSS tokens, React, Tailwind, shadcn)" },
      { label: "Remotion video project", description: "Install brand assets and bootstrap Remotion dependencies" },
      { label: "Presentations", description: "Install brand assets for creating branded PowerPoint decks" }
    ],
    multiSelect: false
  }]
})
```

Store the response as `PROJECT_TYPE` (assets / html / remotion / pptx). If the user picks "Other" (the auto-provided escape hatch), treat it as `assets` and just install the brand-assets without bootstrapping anything else. This determines post-setup guidance and whether Remotion bootstrap runs.

---

## Step 1: Check Latest Version + Existing Config

Run both checks in parallel before doing anything else.

**Check GitHub for the latest release:**
```bash
LATEST_INFO=$(curl -s https://api.github.com/repos/jakerains/elevenlabs-brand-kit/releases/latest)
LATEST_VERSION=$(echo "$LATEST_INFO" | python3 -c "import json,sys; print(json.load(sys.stdin)['tag_name'])" 2>/dev/null)
ASSET_URL=$(echo "$LATEST_INFO" | python3 -c "import json,sys; assets=json.load(sys.stdin)['assets']; print(next(a['browser_download_url'] for a in assets if a['name'].endswith('.zip')))" 2>/dev/null)
echo "Latest version: $LATEST_VERSION"
echo "Download URL: $ASSET_URL"
```

**Check existing config:**
```bash
CONFIG_FILE="$HOME/.elevenlabs-kit/config.json"
if [ -f "$CONFIG_FILE" ]; then
  INSTALLED_VERSION=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE')).get('assetVersion','none'))" 2>/dev/null)
  ASSET_LOCATION=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE')).get('assetLocation','none'))" 2>/dev/null)
  CENTRAL_PATH=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE')).get('centralPath',''))" 2>/dev/null)
  echo "Installed version: $INSTALLED_VERSION  Location: $ASSET_LOCATION"
else
  INSTALLED_VERSION="none"
  ASSET_LOCATION="none"
fi
```

**Decision logic — follow exactly one path:**

| Condition | Action |
|-----------|--------|
| No config (`INSTALLED_VERSION="none"`) | Go to **Step 2** (ask storage preference) |
| Config exists, version matches latest, assets exist at configured location | Tell user assets are current. If this is a new project in central mode, go to **Step 4** to link. Otherwise done. |
| Config exists, version is **outdated** | Tell the user there's an update available and ask if they want to install it (see prompt below). If yes — skip Step 2, use stored `ASSET_LOCATION`, go to **Step 3**. If no — stop. |
| Config exists but assets are missing/broken | Tell the user assets are missing and re-download using stored preference — go to **Step 3**. |

**Update prompt (use when version is outdated).** Use the `AskUserQuestion` tool:

```
AskUserQuestion({
  questions: [{
    question: "Update available: your assets are at v$INSTALLED_VERSION — v$LATEST_VERSION is available. Want me to update now?",
    header: "Update",
    options: [
      { label: "Yes, update now", description: "Download and install the latest brand assets" },
      { label: "No, skip", description: "Keep the current version" }
    ],
    multiSelect: false
  }]
})
```

Wait for the user's response before proceeding.

---

## Step 2: Ask the User Where to Store Assets

**Only reach this step if no config exists (first-time setup).** Use the `AskUserQuestion` tool:

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

Wait for the user's response before proceeding.

---

## Step 3: Download and Extract Assets

### Download the zip

Use `$ASSET_URL` and `$LATEST_VERSION` from Step 1. If Step 1 failed to fetch (no network), fall back to the hardcoded URL below.

```bash
# $ASSET_URL and $LATEST_VERSION set in Step 1
# Fallback if network check failed:
ASSET_URL="${ASSET_URL:-https://github.com/jakerains/elevenlabs-brand-kit/releases/download/v3.1.0/brand-assets-v3.1.zip}"
LATEST_VERSION="${LATEST_VERSION:-2.1.0}"
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

# Also copy fonts to public/fonts/ — required for Remotion's staticFile() font loading
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

---

## Repair / Update Mode

### Update assets to a new version

Run Step 1 — it checks GitHub for the latest version and compares against the installed version automatically. If an update is available, it will prompt before downloading. Central storage only needs one update — all linked projects get it automatically.

### Re-link a project (central mode)

If a project is missing its symlinks but assets exist centrally, run Step 4.

### Switch storage mode

Delete existing assets/symlinks and re-run from Step 2 with the new choice. The config will be updated automatically.

---

## Remotion Project Bootstrap

**Only run this section if `PROJECT_TYPE` is `remotion` (set in Step 0).**

Ensure a package.json exists, install Remotion if missing, and wire up a `studio` script for launching Remotion Studio:

```bash
# 1. Make sure package.json exists — required for npm install to work
if [ ! -f "package.json" ]; then
  echo "No package.json found — initializing..."
  npm init -y
fi

# 2. Install Remotion if not already present
if grep -q '"remotion"' package.json; then
  echo "Remotion already installed — skipping npm install"
else
  echo "Installing Remotion dependencies..."
  npm install remotion @remotion/cli @remotion/transitions @remotion/fonts @remotion/light-leaks zod
fi

# 3. Add the 'studio' script so user can launch Remotion Studio with `npm run studio`
if ! npm pkg get scripts.studio 2>/dev/null | grep -q "remotion studio"; then
  npm pkg set scripts.studio="remotion studio"
  echo "Added 'studio' script — run with: npm run studio"
else
  echo "'studio' script already configured"
fi
```

Then verify TypeScript (only if tsconfig.json exists):
```bash
if [ -f "tsconfig.json" ]; then
  npx tsc --noEmit
fi
```

**After bootstrap, tell the user:**
> Remotion is ready. Launch the studio anytime with `npm run studio` — it'll open Remotion Studio in your browser for previewing and editing compositions.

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

**If Remotion (`PROJECT_TYPE=remotion`):**
> You're ready to build. The typical flow:
> 1. `/elevenlabs-brand-kit:remotion-spec-builder` — plan your scenes (layout, mode, content, duration)
> 2. `/elevenlabs-brand-kit:remotion-builder` — generate the React/TypeScript code from the spec
> 3. `/elevenlabs-brand-kit:remotion-best-practices` — load if you need Remotion API reference while building

**If presentations (`PROJECT_TYPE=pptx`):**
> You're ready to create branded decks. The typical flow:
> 1. `/elevenlabs-brand-kit:eleven-branded-pptx` — create ElevenLabs-branded PowerPoint presentations from the included 30-slide template
> 2. `/elevenlabs-brand-kit:brand` — check brand compliance on your content
> Requires the `/pptx` skill for file tooling.

