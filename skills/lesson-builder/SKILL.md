---
name: eu-lesson-builder
description: "Generate lesson content for ElevenLabs Academy — VO scripts, Remotion specs, Arcade recording specs, and WorkRamp text companions. Use this skill whenever Jake asks to: write a lesson, draft a lesson, create lesson content, build out a lesson, write a VO script, create a text companion, write an Arcade spec, generate WorkRamp content, produce a lesson, or any variation of creating/updating the deliverables for a specific lesson. Also trigger when Jake says 'let's build L1.2', 'draft M3 lesson 4', 'write the text for lesson 2.3', 'create the Arcade plan for L4.1', or references producing any of the three lesson deliverables (VO, spec, text companion). Even casual requests like 'what do we need for L5.2' or 'let's knock out the next lesson' should use this skill."
---

# ElevenLabs Academy — Lesson Content Production Skill

This skill generates the three deliverables for any lesson in the ElevenLabs Academy Agent Builder Certification: the VO script, the visual spec (Remotion or Arcade), and the WorkRamp text companion. It enforces brand voice, PMM positioning, and a consistent production methodology across all lessons.

## Before You Start — Always Do These

1. **Invoke `/elevenagents-pmm`** — Load PMM positioning context. All lesson content must align with the three pillars (Research-backed, Configurable, Trusted), use "we are the source" language, and be voice-first but omnichannel.
2. **Invoke `/elevenlabs-brand`** — Load brand guidelines. All copy must match ElevenLabs tone: confident, warm, not hype-y, not robotic. Use brand terminology correctly ("ElevenLabs" one word, capital E capital L).
3. **Read the Production Guide** — `01 - Certification Program/Planning & Reference/Lesson_Production_Guide.md` — Confirm the lesson's content type (Video, Arcade, or Both), what's been drafted, and production notes.
4. **Read the Module Structure** — `01 - Certification Program/Academy_Module_Structure_Draft.md` — Get context on where this lesson sits in the curriculum, what comes before and after, and what topics it covers.
5. **Check for existing drafts** — Look in `01 - Certification Program/Experiments/v3-compact/` and `01 - Certification Program/Lessons/` for any existing scripts or VO files for this lesson.

## The Three Deliverables

Every lesson produces up to three outputs depending on its content type. The Production Guide specifies which type each lesson is.

### 1. VO Script (Video lessons only)

**What:** A clean, spoken-word-only script. No production notes, no stage directions. Just the words the AI voice reads.

**When:** Only for lessons marked Text+Video or Text+Video+Arcade in the Production Guide.

**How to write it:**
- Write at ~145 words/min conversational pace
- 3 min video ≈ 435 words, 4 min ≈ 580 words
- Lead with voice examples and audio when relevant (this is an audio company)
- Use active voice, second person ("you'll configure," "your agent")
- Avoid jargon without context — define terms on first use
- The credential name is "ElevenLabs Certified ElevenAgent Builder"
- Module titles use landing page names (see reference below)
- Don't say ElevenLabs was "built to solve the conversation volume problem" — it was founded as a research company to solve dubbing
- Voice-first but omnichannel — lead with voice, acknowledge chat/messaging channels

**Output format:**
```
[Clean VO text — no headers, no formatting, no stage directions.
Just paragraphs of spoken words separated by blank lines.
Each paragraph roughly corresponds to a scene or section.]
```

**File:** `M{X}-L{Y}_v{Z}_VO.md`
**Location:** Same directory as the lesson script

### 2. Visual Spec (depends on content type)

The spec is the build blueprint. It branches based on content type:

#### 2a. Remotion Spec (Video lessons)

**What:** Scene-by-scene blueprint for the Remotion build agent.

**When:** Lessons marked Text+Video or Text+Video+Arcade.

**How:** Read `references/remotion-spec-guide.md` for the full format, available layouts, and component library. Key rules:
- Every scene gets a layout, component, duration, VO beat, and Screengrab flag
- Mark `Screengrab: Yes` for architecture diagrams, feature grids, step flows, checklists — anything the learner would reference later
- Mark `Screengrab: No` for title cards, outros, atmospheric/transitional scenes
- Title card always uses `WaveformScene`, outro always uses `OutroScene`
- Transitions: fade, 20 frames, between every scene
- All backgrounds OFF_WHITE (#FAFAFA), monochrome only

**Output:** `10 - Remotion/specs/M{XX}L{YY}v{Z}-spec.md`

**Also invoke the `elevenlabs-remotion` skill** for detailed spec drafting guidance.

#### 2b. Arcade Recording Spec (Arcade lessons)

**What:** A detailed screen recording plan that tells Jake exactly what to capture in the Arcade Mac app, step by step.

**When:** Lessons marked Text+Arcade or Text+Video+Arcade.

**How:** Read `references/arcade-spec-guide.md` for the full format and Arcade capabilities. Key elements per segment:
- Segment name (matches a section of the text companion)
- Starting screen (URL or UI state)
- Click path (step-by-step: click X → panel Y opens → select Z)
- Hotspot placements (where the viewer clicks to advance)
- Callout text (floating explanations over UI elements)
- Spotlight areas (dim everything except the focus region — use for complex UI)
- Chapter steps (section dividers with title + subtitle + nav buttons)
- End state (what the screen looks like when segment is done)
- Estimated step count

**Output format:** Read `references/arcade-spec-guide.md` for the template.

**File:** `01 - Certification Program/Specs/Arcade/M{XX}L{YY}_arcade-spec.md`

### 3. Text Companion (Every lesson)

**What:** The WorkRamp-ready content document. Structured using WorkRamp's native content blocks, ready for copy-paste into the Guide editor.

**When:** Every single lesson gets a text companion.

**How:** Read `references/workramp-text-companion-guide.md` for the full block mapping, visual format, and content patterns. Key principles:
- Mirrors the lesson's visual content in the same section order
- Uses WorkRamp content blocks: Text, Image, Callout (4 types), Accordion, Embed, Flip Card, Divider, Video, Audio, and Knowledge Check blocks
- Every block is tagged with its WorkRamp type so Jake knows exactly what to create in the editor
- Screenshots use amber (#F59E0B) annotation style consistently
- Callout types: Course Connection (Important), Key Insight (Tip), Good to Know (Info), Warning (Warning)
- Arcade demos embed via the Embed block (iframe)
- Videos embed via the Video block (upload)
- Practice section at the end with hands-on exercises
- Next lesson card at the bottom

**Output format:** Read `references/workramp-text-companion-guide.md` for the template.

**File:** `01 - Certification Program/Lessons/Module {X}/M{X}-L{Y}_v{Z}_Text_Companion.md`

## Production Flow

Follow this order for any lesson:

1. **Identify content type** — Check the Production Guide
2. **Write the full lesson script** (with VO, production notes, screen recording directions)
3. **Extract the clean VO** into a separate `_VO.md` file
4. **Write the Remotion spec** (if video) — invoke `elevenlabs-remotion` skill
5. **Write the Arcade recording spec** (if Arcade) — use the Arcade spec guide
6. **Write the text companion** — structured for WorkRamp copy-paste

Always present deliverables to Jake for review before saving. Jake makes all decisions.

## Module Titles (Landing Page Names — Use These)

1. The Platform & Your First Agent
2. Voice & AI Foundations
3. Prompt Engineering
4. Knowledge & Tools
5. Advanced Architecture
6. Deployment
7. Testing & Evaluation
8. Production Operations
- Certification Exam — Prove It (separate, paid)

## Writing Style — Jake's Voice

All lesson content should sound like a warm, knowledgeable person talking to another person. Not a textbook. Not a corporate training deck. Not AI-generated content.

**Talk to a person, not a student:**
- "Every interaction with a person" — NOT "Every interaction your business has with a customer"
- "There are eight modules" — NOT "Eight modules." (too cold, too structural)
- "It's where you build..." — contractions are natural, use them always
- "This is where you're going to live for this training course" — grounding, specific, human

**Natural sentence flow:**
- Vary energy and pacing. Some lines are punchy. Some flow longer.
- Conversational connectors: "And then there's..." / "From there, you go deeper..." / "Let's start with..."
- Forward-looking closings: "Once finished, you'll have all the knowledge and skills to complete and pass the certification test"
- Let exclamation marks land when there's genuine energy

**Avoid:**
- Corporate speak: no "leverage," "utilize," "comprehensive," "robust," "seamless"
- Cold structural openers: no "Module 1 covers..." or "In this lesson, you will learn..."
- AI-isms: no "Let's dive in," "Here's the thing," "It's worth noting that"
- Hedging: no "essentially," "basically," "it's important to understand that"

**Mechanics:**
- Contractions always: "you'll," "it's," "you're," "don't"
- Em dashes for natural pauses — not semicolons or parentheses
- Short paragraphs. White space between thoughts.
- Active voice. Second person. "You'll configure..." not "The configuration will be..."

**Reference:** See `CLAUDE.md` → "Lesson Writing Style — Jake's Voice" for the full style guide with approved reference VO.

## Key Brand/PMM Rules

- **Credential:** "ElevenLabs Certified ElevenAgent Builder"
- **Origin story:** Founded as a research company to solve dubbing. Agents came later.
- **Three pillars:** Research-backed, Configurable, Trusted
- **"We are the source"** — competitors license ElevenLabs models
- **Voice-first but omnichannel** — lead with voice, acknowledge chat/messaging
- **No "one linear path" language** — use "core agents certification path"
- **ElevenLabs** — one word, capital E, capital L. The II is visual only.
- **Tone:** Confident, warm, instructional. Not hype-y. Not robotic. Not salesy.

## Reference Files

Read these as needed — they contain the detailed formats, templates, and block mappings:

| File | When to Read |
|------|-------------|
| `references/workramp-text-companion-guide.md` | When writing any text companion |
| `references/arcade-spec-guide.md` | When writing an Arcade recording spec |
| `references/remotion-spec-guide.md` | When writing a Remotion video spec (also invoke `elevenlabs-remotion` skill) |

## Existing Content Reference

- **ElevenLabs Dev Docs:** `06 - Content Library/Docs & Guides/LLMS-Full ElevenLabsDevDocs.txt` — authoritative source for platform capabilities, API details, agent configuration
- **Arcade Docs:** `06 - Content Library/Docs & Guides/Arcade llms-full.txt` — full Arcade platform documentation
- **Design System Spec:** `01 - Certification Program/Design_System_Spec.md` — Remotion video design system
- **L1.2 Text Companion Mockup:** `01 - Certification Program/Lessons/Module 1/L1.2-text-companion-mockup.html` — visual reference for text companion format
- **PMM Content Punch List:** `01 - Certification Program/PMM_Content_Punch_List.md` — specific content refinements from PMM review
