# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
- Rebrand from "ElevenLabs Academy Plugin" to "ElevenLabs Remotion Kit"
- Update README to reflect current skills (removed lesson-builder and project-context references)
- Update plugin.json name, description, and repo URL
- Consolidate skills: merge lesson-author into lesson-builder, rename remotion-spec to remotion-spec-builder

### Fixed
- Asset-setup: font paths, project type detection, dynamic versioning

## [1.0.0] - April 4, 2026

### Added
- Initial release as ElevenLabs Academy plugin for Claude Code
- 5 skills: asset-setup, remotion-spec, remotion-builder, remotion-best-practices, brand
- V2 design system with hero, content, and hybrid modes
- Brand asset distribution via GitHub Releases (~278MB optimized)
- Central storage support (`~/.elevenlabs-assets/`)
- 8-variant BrandedCard system with darkflat default
