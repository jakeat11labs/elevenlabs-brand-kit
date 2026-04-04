# Remotion Spec Guide

This reference supplements the `elevenlabs-remotion` skill with specific formatting details for lesson video specs. When writing a Remotion spec, also invoke the `elevenlabs-remotion` skill for the full creative guidance and component library.

## Quick Reference

**Full spec template:** `10 - Remotion/LESSON_SPEC_TEMPLATE.md` — Read this for the complete format with all available layouts and established components.

**Design system:** `01 - Certification Program/Design_System_Spec.md` — Colors, typography, animation rules.

**Design reference:** `10 - Remotion/REMOTION_DESIGN_REFERENCE.md` — Visual system patterns and scene layouts.

**Example spec:** `10 - Remotion/specs/experiments/M01L01v4-spec.md` — A completed spec showing the v4 format.

## Spec File Format

Every Remotion spec follows this structure:

```markdown
# M{XX}L{YY}v{Z} — [Lesson Title]

**Module:** [number] — [module name]
**Lesson:** [number] — [lesson name]
**Version:** [number]
**Duration:** ~[total] seconds ([scene count] scenes)
**VO Pace:** ~145 words/min, conversational

---

## Video Summary
[2-3 sentences: what this lesson teaches, key takeaway]

---

## Scene Plan

### Scene 1 — Title Card
| | |
|---|---|
| **Layout** | Split Title Card |
| **Component** | `WaveformScene` |
| **Duration** | 8 seconds |
| **VO Beat** | [First line of VO] |
| **Screengrab** | No |

**Content:**
- **Title:** `[Lesson title]`
- **Subtitle:** `Module [X] — [Module Name]\nAgent Builder Certification`

**Notes:** Always WaveformScene. Cover1.jpg on right.

---

### Scene 2 — [Scene Name]
| | |
|---|---|
| **Layout** | [From available layouts] |
| **Component** | [Existing component or blank for agent to determine] |
| **Duration** | [seconds] |
| **VO Beat** | [The VO line this scene accompanies] |
| **Screengrab** | [Yes / No] |

**Content:**
- **Section Label:** `[1-3 words, uppercase]`
- **Headline:** `[2-6 words]`
- **[Additional fields per layout...]**

**Notes:** [Special behavior, badges, icons, etc.]

---

### Scene N — Outro
| | |
|---|---|
| **Layout** | Split Title Card |
| **Component** | `OutroScene` |
| **Duration** | 10 seconds |
| **VO Beat** | "Up next..." |
| **Screengrab** | No |

**Content:**
- **Lesson Number:** `Lesson [X.Y+1]`
- **Lesson Title:** `[Next lesson's title]`

---

## Scene Flow Summary

| # | Scene | Layout | Duration | Component | Screengrab |
|---|-------|--------|----------|-----------|------------|
| 1 | Title Card | Split Title Card | 8s | WaveformScene | No |
| ... | | | | | |
| N | Outro | Split Title Card | 10s | OutroScene | No |

**Total:** ~[X] seconds
```

## Screengrab Rules

Mark **Screengrab: Yes** for scenes containing:
- Architecture diagrams (become reference cards in text companion)
- Feature grids or comparison cards (at-a-glance reference)
- Step flows or progressions (translate to step sequences in text)
- Checklists or numbered steps (visual summaries)
- Any visual the learner would want to reference during practice

Mark **Screengrab: No** for:
- Title cards and outros
- Atmospheric/transitional scenes (logo reveals, multiplier visuals)
- Scenes only meaningful in motion

**Output path:** `10 - Remotion/screengrabs/M{XX}L{YY}/scene-{N}-{scene-name-slug}.png`
**Resolution:** 1920x1080 (same as video)

## Design Rules (Non-Negotiable)

1. **All light** — every background is OFF_WHITE (#FAFAFA). No dark backgrounds.
2. **Monochrome only** — GRAPHITE, OFF_WHITE, CREAM, LIGHT_GRAY, MID_GRAY, DARK_GRAY. No color.
3. **Transitions** — Fade only, 20 frames, between every scene.
4. **Title + Outro** — Every lesson starts with Split Title Card, ends with Outro.
5. **II Watermark** — Present on every scene except the title card.
6. **Cards fill the frame** — No tiny floating cards.

## Available Layouts

Refer to the template file for the complete list. The most commonly used:

| Layout | Best For | Existing Components |
|--------|----------|-------------------|
| Split Title Card | Openers, outros, section intros | WaveformScene, OriginStory, OutroScene |
| Three-Column Cards | Categories, pillars, 3-item groups | ThreePillarsScene, ChannelStrip, TextRevealScene |
| Three-Step Flow | Journeys, progressions | PlatformEvolution |
| Split List | Module maps, inventories (5-10 items) | TimelineProgression |
| Centered Hero | Welcome moments, key statements | WelcomeCard |
| Connected Rows | Architecture layers, stacked processes | FoundationDiagram |
| Hero Text | Methodology, 3 big concepts | LearnBuildProve |
| Split + Feature Grid | Product deep-dives with 2x2 grid | PillarDetailScene |
| Code Display | API calls, CLI commands | CodeDisplay |
| Numbered Steps | Instructions, processes | NumberedSteps |

## Content Guidelines

- **Headlines:** Short, punchy, 2-6 words.
- **Section labels:** 1-3 words, uppercase. Think "category."
- **Card descriptions:** 1-2 sentences max. On-screen text, not VO scripts.
- Use `\n` for explicit line breaks in titles/subtitles.
- "ElevenLabs" — one word, capital E, capital L. Always.
- VO pacing: ~145 words/min. 3 min = ~435 words, 4 min = ~580 words.
- Each scene's duration should match the VO beat it accompanies.
