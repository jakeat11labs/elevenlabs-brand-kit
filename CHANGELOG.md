# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [4.0.0] - April 27, 2026

### Added
- **`hyperframes`** skill — official HeyGen HyperFrames reference (composition authoring, timing, media, captions, audio-reactive visuals, transitions). Packaged in directly from `~/.agents/skills/hyperframes`, mirroring how the Remotion best-practices skill was bundled.
- **`hyperframes-cli`** skill — official HeyGen CLI reference (init, lint, preview, render, transcribe, tts, doctor, etc.).
- **`hyperframes-spec`** skill — drafts spec markdown for HyperFrames branded videos. Replaces `remotion-spec`. Spec format is implementation-agnostic so existing Remotion specs translate directly.
- **`hyperframes-builder`** skill — builds HTML+GSAP compositions from spec files. Replaces `remotion-builder`. Includes `project-scaffolding.md` for setting up a new HyperFrames project from scratch with brand assets pre-wired.

### Changed
- **Marketplace tags** — replaced `remotion` with `hyperframes`.
- **Manifest description** — replaced "Remotion video production" with "HyperFrames video production (HTML+GSAP)".
- **Version bump** — major (3.x → 4.0.0) because the Remotion skills are deprecated and not the recommended path. Existing Remotion code/specs are still readable by the deprecated skills, but new work goes through HyperFrames.

### Deprecated
- **`remotion-spec`**, **`remotion-builder`**, and **`remotion-best-practices`** skills are now deprecated. Their `SKILL.md` descriptions are prefixed `[DEPRECATED — use hyperframes-* instead]` and a banner at the top of each file directs readers to the HyperFrames replacements. Files are preserved for reference and for editing legacy Remotion projects. They will be removed in a future release.

### Why HyperFrames over Remotion

HyperFrames (HTML+GSAP) gives faster iteration than the Remotion render loop:
- **Studio-driven preview** with hot reload and audio-drag editing
- **Snapshot speed** is dramatically faster than draft renders
- **Per-clip audio** is easier to edit (drag in Studio) than React `<Audio>` components
- **Simpler dependency surface** — one CSS + one JS + GSAP from CDN, no React/TypeScript/Zod build pipeline

The Remotion skills are kept around so existing customer content stays buildable, but all new branded video work goes through HyperFrames.

## [3.2.3] - April 21, 2026

### Changed
- Repo moved from `the previous GitHub namespace` to `github.com/jakeat11labs/elevenlabs-brand-kit`. Plugin manifest, marketplace config, asset-setup download URLs, and all README/skill references updated.
- Brand asset release bumped to `v3.1.1` (filename `brand-assets-v3.1.1.zip`) so the new-repo release is distinct from the old-repo `v3.1.0`. Contents are unchanged from v3.1.0.

### Fixed
- `/elevenlabs-brand-kit:asset-setup` was 404'ing on both the latest-release API and the hardcoded fallback URL after the repo move. Both now resolve against the new repo and the v3.1.1 release.

## [3.2.2] - April 14, 2026

### Added
- Pre-flight check (Step 0) runs silently before asking any questions — detects existing central installs, outdated versions, and already-linked projects
- Quick-link fast path (Path A) — when central assets are already installed and current, just asks project type, symlinks, and bootstraps. No redundant download or storage questions.
- Path-based routing (Step 1) with 5 paths: existing central (A), outdated (B), first-time (C), missing assets (D), already installed (E)
- Central storage now writes `CLAUDE.md` and `AGENTS.md` to `~/.elevenlabs-kit/` — full brand identity, color tokens, typography, icon selection, and compliance checklist so any AI agent that accesses the central store stays on-brand
- Brand compliance check (`/elevenlabs-brand-kit:brand`) invoked automatically after asset setup completes

### Fixed
- Remotion bootstrap: broken `Cannot find module 'react/jsx-runtime'` — now delegates to Remotion's official scaffolder (`create-video`) which installs React peers, TypeScript, config files, and entry points correctly; includes manual fallback with explicit `react react-dom` and dev deps
- Remotion bootstrap: added smoke test (`react/jsx-runtime` resolve + `remotion versions`) to catch install failures before the user runs studio

### Changed
- Asset-setup flow restructured: config check happens before any user interaction, not after. Returning users with central storage get a near-instant setup experience.
- Storage preference only asked for first-time setup (Path C) — returning users with an existing central install skip straight to project type
- Remotion studio launch instructions simplified to `studio`

## [3.2.1] - April 14, 2026

### Added
- Remotion project type now auto-initializes `package.json` if missing, installs Remotion when absent, and wires up a `studio` npm script so users can launch Remotion Studio with `npm run studio`

## [3.2.0] - April 14, 2026

### Added
- Branded PPTX skill (`eleven-branded-pptx`) — create ElevenLabs-branded PowerPoint presentations from the included 30-slide template
- Standalone `.skill` file for Claude Desktop / Cowork Desktop — self-contained branded PPTX with 54 embedded media files
- Presentations project type in asset-setup — guides users to the branded PPTX workflow
- AskUserQuestion UI for all asset-setup prompts — clean selection interface instead of raw text
- Quick-nav badges in README for Claude Code, Claude Desktop, Brand Assets, and Skills
- Marketplace install instructions (`/plugin marketplace add` + `/plugin install`)

### Changed
- Renamed `branded-pptx` skill to `eleven-branded-pptx` across all references
- Plugin source switched from GitHub clone to relative path — avoids redundant re-clone during install
- Plugin manifest `repository` field uses string format per Claude Code schema

### Fixed
- Plugin install failure caused by SSH clone attempt on public repo (permission denied publickey)
- `.skill` download links now point to GitHub release assets
- Template file renamed to remove special characters that broke downloads
- Dark mode logo rendering in README
- Asset-setup references updated to v3.1.0 release URL

## [2.0.0] - April 10, 2026

### Added
- New `branded-web` skill — guidance for building ElevenLabs-branded web experiences (HTML, CSS, React, Tailwind, shadcn/ui)
- 7 web-specific rule files: css-tokens, tailwind-setup, shadcn-theming, card-variants, backgrounds, typography-web, voice-orbs-web

### Changed
- **BREAKING:** Rebrand from "ElevenLabs Remotion Kit" to "ElevenLabs Brand Kit"
- **BREAKING:** Plugin name changed from `elevenlabs-remotion-kit` to `elevenlabs-brand-kit` — all skill commands now use `/elevenlabs-brand-kit:` prefix
- README restructured with brand-first positioning; Remotion skills presented as one workflow
- brand-assets/CLAUDE.md updated to be framework-agnostic with web usage examples
- brand-assets/index.html catalog updated: "Remotion Catalog" → "Brand Catalog", copy payloads now show both web and Remotion paths
- asset-setup SKILL.md updated with web-first guidance and new plugin name
- Brand skill: clarified Core Brand vs. Monochrome mode — educational/training content uses Core Brand (not Monochrome)

### Optimized
- Brand asset images resized to max 1920px, JPEG quality 85, 72 DPI — archive reduced from ~278MB to ~75MB
- Removed legacy `images/` directory (superseded by `backgrounds/gradient/`)
- `seamless-texture.jpg` reduced from 4096x4096 (25MB) to 1024x1024 (~1MB)

### Unchanged
- All Remotion skills (remotion-spec, remotion-builder, remotion-best-practices) preserved without modification

## [1.1.0] - April 9, 2026

### Changed
- Rebrand from "ElevenLabs Plugin" to "ElevenLabs Remotion Kit"
- Update README to reflect current skills (removed lesson-builder and project-context references)
- Update plugin.json name, description, and repo URL
- Consolidate skills: merge lesson-author into lesson-builder, rename remotion-spec to remotion-spec-builder

### Fixed
- Asset-setup: font paths, project type detection, dynamic versioning

## [1.0.0] - April 4, 2026

### Added
- Initial release as ElevenLabs plugin for Claude Code
- 5 skills: asset-setup, remotion-spec, remotion-builder, remotion-best-practices, brand
- V2 design system with hero, content, and hybrid modes
- Brand asset distribution via GitHub Releases (~278MB optimized)
- Central storage support (`~/.elevenlabs-assets/`)
- 8-variant BrandedCard system with darkflat default
