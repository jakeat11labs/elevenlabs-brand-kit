---
name: asset-setup
description: "Download ElevenLabs brand assets (backgrounds, icons, fonts, voice orbs, logos) and bootstrap a project. Use when someone says 'setup assets', 'download brand assets', 'get the assets', 'bootstrap', 'install assets', 'new project setup', 'asset setup', or when brand assets are missing. Also handles updating assets to a newer version."
---

# ElevenLabs Academy — Asset Setup

This skill downloads the official ElevenLabs brand asset pack and sets up a project to use them. It supports two storage modes: **project-local** (assets live inside the project) or **central** (single shared copy across all projects).

## Overview

Brand assets are distributed as a versioned zip from GitHub Releases. The zip contains:
- `brand-assets/` — backgrounds, icons, voice orbs, logos, diagrams, screenshots, images, color tokens, brand guidelines, and visual catalogs
- `fonts/` — KMR Waldenburg in 4 weights (Buch, Normal, Halbfett, Fett)

**Current version:** `v2.1.0`
**Download URL:** `https://github.com/jakerains/elevenlabs-remotion-kit/releases/download/v2.1.0/brand-assets-v2.1.zip`

---

## Step 1: Check Latest Version + Existing Config

Run both checks in parallel before doing anything else.

**Check GitHub for the latest release:**
```bash
LATEST_INFO=$(curl -s https://api.github.com/repos/jakerains/elevenlabs-remotion-kit/releases/latest)
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

**Update prompt (use when version is outdated):**

> **Update available:** your assets are at v`$INSTALLED_VERSION` — v`$LATEST_VERSION` is available.
>
> Changes in this release are in the GitHub changelog. Want me to update now? (yes / no)

Wait for the user's response before proceeding.

---

## Step 2: Ask the User Where to Store Assets

**Only reach this step if no config exists (first-time setup).** Present the two options clearly:

> **Where should I store the brand assets?**
>
> 1. **Project-local** — Downloads assets directly into this project's `public/` directory. Each project gets its own copy. Best if you only work on one project at a time.
>
> 2. **Central (shared)** — Downloads assets once to `~/.elevenlabs-kit/` and creates symlinks in each project. All projects share the same assets — saves disk space and keeps everything in sync. Best if you work across multiple ElevenLabs projects.
>
> Which do you prefer? (1 or 2)

Wait for the user's response before proceeding.

---

## Step 3: Download and Extract Assets

### Download the zip

Use `$ASSET_URL` and `$LATEST_VERSION` from Step 1. If Step 1 failed to fetch (no network), fall back to the hardcoded URL below.

```bash
# $ASSET_URL and $LATEST_VERSION set in Step 1
# Fallback if network check failed:
ASSET_URL="${ASSET_URL:-https://github.com/jakerains/elevenlabs-remotion-kit/releases/download/v2.1.0/brand-assets-v2.1.zip}"
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

# Copy assets into project
cp -r /tmp/elevenlabs-assets-extract/brand-assets/* public/brand-assets/
cp -r /tmp/elevenlabs-assets-extract/fonts/* public/fonts/

# Clean up
rm -rf /tmp/elevenlabs-assets-extract "$TEMP_ZIP"

echo "Brand assets installed to public/brand-assets/"
echo "Fonts installed to public/fonts/"
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

# Create symlinks to central store
ln -s "$HOME/.elevenlabs-kit/brand-assets" public/brand-assets
ln -s "$HOME/.elevenlabs-kit/fonts" public/fonts

echo "Symlinked public/brand-assets → $CENTRAL_DIR/brand-assets/"
echo "Symlinked public/fonts → $CENTRAL_DIR/fonts/"
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

if [ -d "$CENTRAL_DIR/brand-assets" ] && [ -d "$CENTRAL_DIR/fonts" ]; then
  mkdir -p public
  rm -rf public/brand-assets public/fonts
  ln -s "$CENTRAL_DIR/brand-assets" public/brand-assets
  ln -s "$CENTRAL_DIR/fonts" public/fonts
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

```bash
CONFIG_FILE="$HOME/.elevenlabs-kit/config.json"
CURRENT_VERSION=$(python3 -c "import json; print(json.load(open('$CONFIG_FILE'))['assetVersion'])" 2>/dev/null || echo "unknown")
echo "Current version: $CURRENT_VERSION"
echo "Available version: 2.1.0"
```

If versions differ, re-run Step 3 using the stored `assetLocation` preference. Central storage only needs one update — all linked projects get it automatically.

### Re-link a project (central mode)

If a project is missing its symlinks but assets exist centrally, run Step 4.

### Switch storage mode

Delete existing assets/symlinks and re-run from Step 2 with the new choice. The config will be updated automatically.

---

## Remotion Project Bootstrap

If this is a brand new Remotion project (not just adding assets to an existing project), also install dependencies:

```bash
npm install remotion @remotion/cli @remotion/transitions @remotion/fonts @remotion/light-leaks zod
```

Then verify TypeScript:
```bash
npx tsc --noEmit
```

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

This file is read by all ElevenLabs Remotion Kit skills to locate brand assets. When `assetLocation` is `"central"`, skills resolve asset paths through `centralPath` instead of expecting them in the project directory.

---

## After Setup

**Open the catalog:** `public/brand-assets/index.html` in any browser. It shows every background, icon set, voice orb, scene component, layout, and color token — all with one-click copy buttons that output structured markdown ready to paste into an agent prompt. Use this instead of hunting through file directories.

Use the other plugin skills:
- `/elevenlabs-remotion-kit:remotion-spec-builder` — Draft a video spec
- `/elevenlabs-remotion-kit:remotion-builder` — Build the composition from a spec
- `/elevenlabs-remotion-kit:elevenlabs-brand` — Enforce brand guidelines
- `/elevenlabs-remotion-kit:remotion-best-practices` — Remotion API reference
