---
name: eu-project-context
description: "Project context for ElevenLabs University and the ElevenAgents Certification. Use this skill for ANY task related to the university, certification, curriculum, lessons, modules, WorkRamp, learner experience, or ElevenLabs educational content. This includes: writing or editing lesson scripts, creating VO scripts, updating module structure, referencing module titles, using the correct credential name, checking project status, working with the curriculum tracker, updating stakeholder profiles, or any content creation for the cert. Also trigger when Jake mentions university, cert, certification, module, lesson, WorkRamp, landing page, or any of the 10 module names (The Platform, Voice & AI Foundations, Your First Agent, Prompt Engineering, Knowledge & Tools, Advanced Architecture, Deployment, Testing & Evaluation, Production Operations, Prove It). Even casual references like 'update the lesson' or 'check module 3' should use this skill."
---

# ElevenLabs University — Project Context Skill

This skill front-loads the critical decisions, naming conventions, and project rules so any model can work on the ElevenLabs University project without losing context. Read this before doing any work.

For full project context, read `CLAUDE.md` in the workspace root. This skill is the quick-reference cheat sheet.

## The Project

Jake Rains (Customer Enablement, ElevenLabs) is building ElevenLabs University and the ElevenAgents Certification from scratch. The goal is V1 — a core agents certification path that takes learners from zero to certified.

## Critical Names — Get These Right Every Time

**Credential name:** ElevenLabs Certified ElevenAgent Builder
- NOT "Certified Agent Builder" — must include "ElevenAgent"
- NOT "ElevenLabs Agent Builder Certification" — the credential is what they EARN, not the course name

**Company name:** ElevenLabs (one word, capital E, capital L)
- NOT "Eleven Labs" (no space)
- NOT "ElevenLabs" with the II prefix in text

**Product names:**
- ElevenAgents (the conversational AI platform)
- ElevenCreative (the studio — TTS, dubbing, sound effects)
- ElevenAPI (the developer layer — REST API, SDKs, CLI)

## 10 Module Titles — Use These, Not Internal Names

These are the landing page titles. Always use these in any learner-facing or public content.

| # | Title | Subtitle |
|---|---|---|
| 1 | **The Platform** | ElevenLabs & the AI Voice Landscape |
| 2 | **Voice & AI Foundations** | How the Technology Works |
| 3 | **Your First Agent** | Create, Configure & Test |
| 4 | **Prompt Engineering** | LLM Configuration & Conversation Design |
| 5 | **Knowledge & Tools** | Connect Your Agent to the World |
| 6 | **Advanced Architecture** | Workflows, API & Multi-Agent Systems |
| 7 | **Deployment** | Phone, Web, Mobile & Enterprise Channels |
| 8 | **Testing & Evaluation** | Scenarios, Experiments & CI/CD |
| 9 | **Production Operations** | Analytics, Cost, Privacy & Scale |
| 10 | **Prove It** | Certification |

Internal/casual names like "Here's Who We Are" or "Make It Smart" are fine for Jake's internal shorthand but should NEVER appear in lesson content, landing pages, or any deliverable.

## Key Decisions — Locked

These are settled. Don't revisit unless Jake explicitly says to.

- **Voice-first but omnichannel:** Lead with voice, acknowledge chat/messaging channels. ElevenAgents does voice + chat + WhatsApp + email. Don't feed the "we only do voice" perception gap.
- **All-light design:** All Remotion video compositions use OFF_WHITE (#FAFAFA) backgrounds. GRAPHITE (#1E1916) for text and graphic elements only, never as a background. See `01 - Certification Program/Design_System_Spec.md`.
- **No "one linear path" language:** We may add partner, enterprise, or advanced tracks later. Use "the core agents certification path" or similar.
- **No ElevenCreative or ElevenAPI expansion tracks:** Future tracks are all agents-focused.
- **ElevenLabs origin story:** Founded as a research company to solve dubbing. Agents came later as the technology evolved. Do NOT say "ElevenLabs was built to solve the conversation volume problem."
- **PMM alignment:** Three pillars: Research-backed, Configurable, Trusted. "We are the source" — competitors license our models. Use the `elevenagents-pmm` skill for positioning.
- **Native WorkRamp content only — no SCORM:** All content built as Guides + Resources. WorkRamp may deprecate SCORM.
- **Training = FREE. Certification exam = PAID** (behind Stripe paywall).

## Instructional Design: LEARN → BUILD → PROVE

Every module and every lesson follows this rhythm:
- **Learn** — Concepts, walkthroughs, examples
- **Build** — Hands-on exercise in the actual ElevenLabs platform
- **Prove** — Knowledge check or assessment before moving on

This applies at lesson level AND module level. Module 10 is the ultimate PROVE — build a real agent end-to-end for a use case they haven't seen before.

## Content Creation Rules

When writing or editing any cert content:

1. Check `01 - Certification Program/Design_System_Spec.md` for visual/Remotion content
2. Check `01 - Certification Program/University_Module_Structure_Draft.md` for where content fits
3. Use landing page titles (see table above), not casual internal names
4. Credential name: "ElevenLabs Certified ElevenAgent Builder"
5. Voice-first but omnichannel — lead with voice, acknowledge chat/messaging
6. Version files — create new versions (v2 → v3) rather than overwriting
7. Lesson file naming: `M{module}-L{lesson}_v{version}_Title.md`
8. VO script naming: `M{module}-L{lesson}_v{version}_VO.md`

## Three Audiences

The cert is built for:
1. **Developers & Builders** — building agents into products or shipping for clients
2. **Enterprise Teams** — deploying ElevenAgents across their organization
3. **Partners & Agencies** — delivering agent solutions to customers

## WorkRamp Terminology

| Our Term | WorkRamp Term |
|---|---|
| Module | Guide |
| Lesson | Task (task page) |
| Section heading | Section |
| Full cert track | Path |
| Certification exam | Certification |
| Landing/overview page | Custom Page |

## Key Files

- `CLAUDE.md` — full project context, workflows, stakeholder info
- `TODO.md` — running task list
- `01 - Certification Program/University_Module_Structure_Draft.md` — master module structure
- `01 - Certification Program/Curriculum_Tracker.xlsx` — spreadsheet tracker
- `01 - Certification Program/Design_System_Spec.md` — Remotion video design system
- `01 - Certification Program/PMM_Content_Punch_List.md` — content refinements from PMM review
- `04 - Stakeholders & Meetings/Stakeholder_Profiles.md` — who's who
- `06 - Content Library/Docs & Guides/Wiki_Reference.md` — internal wiki pages

## Related Skills

- `elevenlabs-brand` — brand guidelines (colors, typography, logo rules)
- `elevenagents-pmm` — product marketing positioning and messaging
- `elevenlabs-remotion` — on-brand Remotion video creation
