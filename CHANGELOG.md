# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
- Central storage support (`~/.elevenlabs-academy/`)
- 8-variant BrandedCard system with darkflat default
