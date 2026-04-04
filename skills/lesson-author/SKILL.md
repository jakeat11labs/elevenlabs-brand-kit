---
name: eu-lesson-author
description: "Author lesson content for ElevenLabs University certification. Use this skill whenever writing, drafting, or producing lesson scripts, VO scripts, or text companions for the certification program. Triggers include: 'write lesson X.Y', 'draft lesson', 'author lesson', 'create a lesson script', 'flesh out the skeleton', 'turn this into a script', 'production doc', 'let's do M3-L2', 'build out lesson', or any reference to producing content for a specific module/lesson number. Also trigger when given a lesson entry from the skeleton and asked to produce a full script. Do NOT trigger for: meeting notes processing, Remotion spec creation (use elevenlabs-remotion), WorkRamp platform configuration, or general project management."
---

# ElevenLabs University — Lesson Author

This skill is the institutional knowledge for creating high-quality, consistent lesson content across the ElevenLabs University certification program. It encodes instructional design methodology, content quality standards, production format, and the text companion building block system so every lesson is pedagogically sound and structurally consistent.

Every lesson produces TWO deliverables:
1. A **lesson script** — VO narrative + production directions (Remotion callouts, screen recording cues) + text companion content
2. A **clean VO script** — extracted narration for voice recording

The `elevenlabs-remotion` skill handles Remotion motion graphics specs separately. This skill handles the instructional design, writing, and content structure.

---

## A. Input Requirements

Before authoring, gather these inputs:

### Required
1. **Lesson identifier** — M#-L# format (e.g., M3-L1, M5-L2)
2. **Skeleton entry** — Read the lesson's entry from `01 - Certification Program/Planning & Reference/Lesson_Skeleton_Draft.md`. Extract: title, type, duration, what-it-covers, practice, comprehension check.

### Auto-derived (don't ask — look these up)
3. **Module position** — Which module, what came before, what comes after. Read from the skeleton.
4. **Prior knowledge** — What the learner has already built and learned. Derive from previous lesson entries.
5. **Scaffolding level** — Determined by module number (see Section C).

### Optional (ask if unclear)
6. **Version number** — Default to v1 unless iterating on an existing script.
7. **Specific focus areas** — Jake may want to emphasize certain aspects of the skeleton entry.

---

## B. Instructional Design Framework

Every lesson must satisfy these frameworks. They should be woven NATURALLY into the content — never labeled as "STEP 1: GAIN ATTENTION." The learner experiences them without seeing the scaffolding.

### Gagne's Nine Events

Verify each is present before delivering the lesson script:

1. **Hook** — Open with a callback to something the learner built, a real-world problem, or a "what if" scenario. Never open with "In this lesson you'll learn..." Start with why this matters.

2. **Objectives** — State what the learner will be able to DO after this lesson. Use observable action verbs: configure, build, test, compare, deploy, design, evaluate, create. Never use: understand, learn, know, appreciate, be familiar with.

3. **Prior recall** — Explicitly reference something from a previous lesson or module. "Remember when you configured your agent's system prompt? Now we're going to..." This anchors new content to existing knowledge.

4. **Content presentation** — The core instruction. Chunk into discrete sections, each covering ONE concept or skill. Within each section: explain what it is, show how it works, then show when/why to use it.

5. **Guided learning** — Worked examples, walkthroughs, demonstrations. Every concept needs visible proof — a screen recording, diagram, before/after comparison, or live demo. No concept exists only as text.

6. **Practice** — Where the lesson type calls for it, the learner DOES something. Not watching, not reading — doing. Practice should mirror a real task they'd face in production.

7. **Feedback** — Show consequences of choices, not just correct/incorrect. "If you set temperature too high, your agent will give inconsistent answers — try it and see." When writing comprehension checks, include brief rationales for correct and incorrect answers.

8. **Assessment** — Comprehension check questions for heavier concept lessons. Always multiple choice (A/B/C/D). 1-2 questions max. See Section J for format.

9. **Bridge** — Connect to the next lesson and to real-world application. "Now that you've configured guardrails, your agent is production-safe. Next, we'll give it knowledge and tools so it can actually help people." End every lesson by pointing forward.

### Merrill's First Principles

For each lesson, answer these questions during planning:

- **Problem-centered:** What real-world task does this lesson help the learner accomplish? (e.g., "Make your agent stop hallucinating" not "Learn about guardrails")
- **Activation:** What does the learner already know that connects to this? What did they build in previous lessons?
- **Demonstration:** Where is the concept shown, not just explained? Is there a screen recording, diagram, or worked example?
- **Application:** Where does the learner practice IN CONTEXT? (MC quizzes alone are insufficient — the learner must do the actual work where the lesson type calls for it)
- **Integration:** How does the learner adapt this to their OWN use case? (e.g., "Go back to YOUR agent and configure this for YOUR scenario")

### Action Mapping Filter

Apply this filter to every piece of content:

| If the content is... | Then... |
|---|---|
| **Must-know** — directly supports a learning objective, learner needs this to succeed | Include in the main flow |
| **Should-know** — helpful context, makes the learner more capable but not blocked without it | Put in an accordion or "Good to Know" callout |
| **Nice-to-know** — interesting but not actionable, background info, trivia | Cut it. Or link to docs. Don't teach it. |

Ask: "Does this help the learner DO something they need to do?" If the answer is "it's interesting," it doesn't belong in the main flow.

### Scaffolding Progression (4C/ID)

Keyed to module number. This determines how much hand-holding the lesson provides:

| Modules | Scaffolding Level | What it looks like |
|---|---|---|
| **M1-M2** | Maximum | Every click shown. Step-by-step screen recordings of full workflows. "Click the Configure tab, then click Knowledge Base, then click Add Document." Detailed annotations on every screenshot. |
| **M3-M4** | Medium | Key steps shown, routine navigation assumed. "Open the system prompt panel and add a Guardrails section with these three rules." Assume they can navigate the dashboard. |
| **M5-M6** | Goal-oriented | Steps outlined but not hand-held. "Build a webhook tool that queries an order lookup API. Configure authentication and test the response." They know how to find things. |
| **M7-M8** | Minimal | Complex scenario, light guidance. "Design a test suite covering your three highest-risk conversation paths. Include both scenario and tool call tests." They're near-certified. |

This progression OVERRIDES lesson type. A Concept lesson in M1 gets maximum scaffolding. A Concept lesson in M8 gets minimal. The module position determines learner competence.

---

## C. Lesson Type Patterns

Select the pattern based on the lesson's Type field in the skeleton. Most lessons are hybrids.

### Concept Lesson

Structure:
1. Hook — real-world problem or callback to what they've built
2. Learning objectives (2-3)
3. **Section 1:** Core concept with architecture diagram or concept map
4. **Section 2:** How it works — mechanism, process, flow
5. **Section 3:** When and why — decision framework, tradeoffs, comparison table
6. Recap with reference card
7. Comprehension check (1-2 MC questions) — for heavier concept lessons
8. Bridge to next lesson

**Production notes:** Heavier on Remotion motion graphics (concept diagrams, comparison cards, architecture visuals). Fewer screen recordings. Reference cards and comparison tables in text companion.

**VO tone suggestion:** Reflective, explanatory, building understanding. "Here's what's happening under the hood..." "The reason this matters is..."

### Walkthrough Lesson

Structure:
1. Hook — what we're about to see/do and why it matters
2. Learning objectives (2-4)
3. **Sections 1-N:** Each section is a walkthrough segment:
   - Brief context (1-2 sentences: what and why)
   - Screen recording direction (what to show, where to navigate)
   - Step sequence (numbered steps matching the recording)
   - Annotated screenshot callouts
4. Recap
5. Practice exercise (lightweight — apply what you just watched)
6. Bridge to next lesson

**Production notes:** Heavy screen recordings with alpha overlays (lower thirds, highlight boxes, callout labels). Remotion compositions for intro/section breaks/outro only. Step sequences and annotated screenshots dominate the text companion.

**VO tone suggestion:** Direct, step-oriented, guiding. "Now click here..." "You'll see this panel open..." "Watch what happens when we..."

### Hands-On Lesson

Structure:
1. Hook — what you're about to build and the outcome
2. Learning objectives (2-4)
3. **Worked example** — Watch the instructor build it (screen recording + VO). Complete walkthrough.
4. **Guided practice** — Learner builds a similar one with guidance. Steps provided but not every click.
5. **Independent practice** — Learner builds on their own with goal only. "Now do this for YOUR use case."
6. Debrief — what to check, common mistakes, what good looks like
7. Bridge to next lesson

**Production notes:** Screen recording of the worked example, then the text companion carries the guided + independent practice with step sequences and practice checklists. Remotion compositions for concept setup if needed.

**VO tone suggestion:** Encouraging, coach-like. "Your turn..." "Try this..." "Don't worry if it's not perfect — we'll refine it in the next lesson."

### Hybrid Lessons (most common)

For types like "Concept -> Walkthrough" or "Walkthrough -> Hands-On":
- Follow the first type's pattern for the opening sections
- Insert a clear transition: "Now let's put this into practice" / "Let's see this in action"
- Follow the second type's pattern for the remaining sections
- The transition point should feel natural, not jarring

---

## D. Lesson Script Output Format

Every lesson script follows this exact format (derived from the v4 exemplar scripts):

```markdown
# Module X, Lesson Y: [Title] (v[version])

**Module:** X — [Module Landing Page Title]
**Lesson:** X.Y — [Lesson Title]
**Type:** [Concept | Walkthrough | Hands-On | Concept -> Walkthrough | etc.]
**Duration:** ~X min video, ~Y min text read
**Format:** ElevenLabs AI voiceover + Remotion motion graphics [+ screen recordings with alpha overlays], assembled in Premiere Pro

---

## Learning Objectives

By the end of this lesson, learners will be able to:
1. [Action verb] + [observable outcome]
2. [Action verb] + [observable outcome]
3. [Action verb] + [observable outcome] (if needed)

---

## Production Notes

- **Voiceover:** ElevenLabs AI voice — [tone notes for this specific lesson]
- **Pacing:** [pacing guidance]
- **Music:** [music direction]
- **Screen capture notes:** [if applicable]

---

## VIDEO SCRIPT

### [SECTION NAME] [timestamp range]

> **[REMOTION: `CompositionID` — Description. Layout, content, animation notes.]**

OR

> **[SCREEN RECORDING: What to show. Duration. Overlay specs (lower thirds, highlight boxes, callout labels).]**

**VO:**

[Narration text in plain conversational prose. 2-4 sentences per section beat.]

---

[Repeat for each section]

---

## Remotion Composition Specs

[Per-composition technical specs if authoring them here, OR:]
**Hand off to `elevenlabs-remotion` skill for detailed specs.**

## Composition Summary

| Composition ID | Duration | Type | Key Visual |
|---|---|---|---|
| `M0XL0Y-Name` | X sec | Remotion / Screen Rec | Description |

## Screen Recording List

| Recording | What to Show | Est. Duration | Overlays Needed |
|---|---|---|---|
| [Name] | [Navigation path + actions] | ~X min | [Lower thirds, highlights, callouts] |

---

## TEXT COMPANION

[See Section F for the full building block specification. The text companion mirrors the video section structure exactly.]

### [Section heading matching video section]

[Content using building blocks: annotated screenshots, step sequences, callouts, accordions, reference cards, diagrams]

...

### Practice

[Numbered checklist matching the practice from the skeleton entry]

### Up Next

> **Lesson X.Y — [Next Lesson Title]**
> [One sentence teaser of what's next]
```

---

## E. Production Notes & VO Guidance

Production notes are **structural, not prescriptive.** Flag what kind of visual treatment each section needs:

### Visual Treatment Types

| Cue | What it means | When to use |
|---|---|---|
| `> **[REMOTION: ...]**` | Motion graphic composition. Concept diagram, card layout, visual metaphor. | Concept explanations, architecture diagrams, comparison visuals, intro/outro |
| `> **[SCREEN RECORDING: ...]**` | Captured dashboard/UI walkthrough with overlay annotations. | Step-by-step demonstrations, platform navigation, configuration flows |
| `> **[DIAGRAM: ...]**` | Static concept diagram for the text companion (may also be a Remotion composition). | Architecture flows, decision trees, component relationships |
| `> **[BEFORE/AFTER: ...]**` | Side-by-side comparison showing a change. | Configuration improvements, prompt refinements, output quality changes |

### VO Tone by Lesson Type

General baseline: conversational, not corporate. First person ("you," "we," "let's"). Confident but not arrogant. Expert colleague showing you something, not a training video narrator.

Beyond the baseline, let the lesson type and content guide the voice:

- **Concept lessons** — more reflective, building understanding. "Here's what's happening..." "The reason this matters is..."
- **Walkthrough lessons** — more direct, guiding through steps. "Now click here..." "Watch what happens..."
- **Hands-on lessons** — encouraging, coach-like. "Your turn..." "Try this..." "Don't worry if..."
- **Advanced modules (M5-M8)** — more peer-to-peer, less instructional. "You know the pattern — now apply it to..."
- **Early modules (M1-M2)** — warmer, more welcoming. "Let's get you oriented..." "This is where everything starts..."

Don't force a tone. Let the content breathe. If a lesson needs to be serious (guardrails, compliance), be serious. If a lesson is exciting (first agent test call), let that energy come through.

---

## F. Text Companion Specification

The text companion is a visual 1:1 of the video — same sections, same order, same content. Not a transcript. Not a reference card. A visual narrative that mirrors the video experience.

**Section mapping rule:** If the video has Section 1 (Architecture), Section 2 (Configuration), Section 3 (Testing) — the text has the same three sections in the same order with the same headings.

### Building Blocks

Use these WorkRamp-native elements consistently:

#### 1. Hero Context (required — every lesson)
- Lesson title (large heading)
- Meta info: duration badge, type badge (Concept / Walkthrough / Hands-On), one-liner subtitle
- Learning objectives in a numbered grid (2-column on desktop)
- Orients the learner immediately — they know what they're getting into

#### 2. Annotated Screenshots
- Every major UI moment gets a screenshot with visual annotations
- **Max 3-4 annotations per image** — more than that is visual overload
- Use amber (#F59E0B) for highlight boxes and callout markers
- Numbered callouts correspond to step numbers in the text
- Prefer multiple small screenshots distributed throughout (not one large image)
- CREAM-background caption below each screenshot explaining what you're seeing
- Tight cropping with enough context for orientation
- Spec format: `[SCREENSHOT: What to capture. Annotations: 1. Element X, 2. Element Y]`

#### 3. Short GIFs (5-10 seconds, looping)
- For multi-click flows where static screenshots fail
- "Click this -> this panel opens -> select this -> result appears"
- Captured from screen recordings, exported as GIF
- Spec format: `[GIF: Start state -> actions -> end state. ~Xs loop.]`

#### 4. Step Sequences
- Numbered steps with a connecting visual line
- Each step: **title** (bold, 1 line) + description (1-2 sentences) + optional inline screenshot
- Use for any "do this, then this, then this" flow
- The 1:1 translation of screen recording walkthroughs

#### 5. Callout Boxes (4 types — use consistently)

| Type | Border Color | Use When | Example |
|---|---|---|---|
| **Course Connection** | Graphite | Linking to content in other modules | "You'll configure advanced guardrails in Module 3" |
| **Key Insight** | Green | Important takeaway, tip, the thing to remember | "Flash v2.5 is the go-to for multilingual agents" |
| **Good to Know** | Blue | Helpful context that's not essential, can be skipped | "Scribe v2 also supports entity detection for 56 types" |
| **Warning** | Amber | Pitfalls, gotchas, things that will break | "Don't set temperature above 0.8 for customer-facing agents" |

WorkRamp native equivalents: Important, Info, Tip, Warning.

#### 6. Reference Cards
- Clean visual summaries: sidebar maps, comparison tables, architecture diagrams, decision frameworks
- Should be "screenshottable" — the thing a learner would save for their notes
- Use for: model comparison tables, tool type decision trees, configuration cheat sheets

#### 7. Accordions
- Expandable sections for deeper detail
- Keeps the main flow clean — advanced detail is one click away
- Use for: setting-by-setting breakdowns, advanced options, "what does each setting do" expansions, edge cases, troubleshooting
- WorkRamp supports accordion objects natively

#### 8. Architecture / Concept Diagrams
- Static versions of Remotion motion graphics
- Labeled, clear, minimal — not cluttered
- Spec format: `[DIAGRAM: What it shows. Layout (horizontal flow / vertical stack / hub-and-spoke). Labels. Connections.]`

#### 9. Practice Section (required if skeleton entry has practice)
- Numbered checklist at the end of the lesson
- Each item is an action: "Create a new agent from blank template" not "Do you understand agent creation?"
- Matches the practice prompt from the video outro
- Progressive: starts with the direct task, ends with "adapt for your own use case"

#### 10. Next Lesson Card
- Dark (GRAPHITE background) footer
- "Up Next: Lesson X.Y — [Title]"
- One sentence teaser: what they'll do/learn next
- Creates continuity between lessons

### Progressive Disclosure Rule

| Content level | Treatment |
|---|---|
| **Must-know** | Visible in the main flow. Always on screen. |
| **Should-know** | In accordions. One click to expand. |
| **Could-know** | In "Good to Know" callouts or linked to docs. |

---

## G. Clean VO Script Extraction

After producing the lesson script, extract a clean VO script:

1. Strip ALL Remotion cues, screen recording directions, production notes, section headers
2. Keep ONLY the narration text in reading order
3. Output as a separate file: `M{module}-L{lesson}_v{version}_VO.md`
4. No formatting, no headers — just the words to be spoken, paragraph by paragraph
5. Add a word count at the bottom for pacing verification
6. Add a duration estimate: word count / 145 = approximate minutes

---

## H. Content Quality Rules

Hard rules — enforce these on every lesson:

### Writing
- **Headings:** Descriptive actions, not clever labels. "Configure the Backup LLM Chain" not "Safety Net." "Test Your Agent in the Playground" not "Taking It for a Spin."
- **Paragraphs:** One idea per paragraph. 2-4 sentences max. Short paragraphs.
- **Bold:** Key terms bold on first use within a lesson only. Never bold for emphasis in running text — use callout boxes instead.
- **Lists:** Use for 3+ items. Bullets for unordered, numbers for sequential steps.
- **Inverted pyramid:** Most important information first in every section. Lead with the answer, then context.

### Accuracy & Naming
- **Product names:** ElevenAgents, ElevenCreative, ElevenAPI (one word each, capital letters). Never "the Agents platform" or "Eleven Agents" (with space).
- **Credential:** "ElevenLabs Certified ElevenAgent Builder" — exact, every time.
- **Company:** ElevenLabs (one word, capital E, capital L). Never "Eleven Labs."
- **Voice-first-plus:** Lead with voice examples, acknowledge chat/messaging channels. Never say ElevenLabs is "voice only."
- **Origin story:** ElevenLabs started as a research company to solve dubbing. NOT "to solve the conversation volume problem."
- **Module references:** Use landing page titles. "The Platform & Your First Agent" not "Module 1" or "Here's Who We Are."
- **Technical accuracy:** Verify claims against `06 - Content Library/Docs & Guides/LLMS-Full ElevenLabsDevDocs.txt` when referencing specific platform capabilities, limits, or features.

### What to Avoid
- **No marketing copy:** No superlatives ("best in class"), no hype ("revolutionary"), no unsubstantiated claims. State capabilities, show evidence.
- **No jargon without context:** Define terms on first use. Don't assume the learner knows what "RAG" or "MCP" means until you've told them.
- **No "understand" objectives:** Learning objectives must use observable verbs. A learner can't demonstrate "understanding" — they can demonstrate configuring, building, comparing, explaining.

---

## I. Module Quiz & Comprehension Check Standards

### Comprehension Checks (within lessons)
- **When:** Heavier concept lessons only. Not every lesson gets one.
- **Format:** Always multiple choice — A, B, C, D options. No free-text. No click-to-reveal.
- **Count:** 1-2 questions max per lesson.
- **Question types:** Application ("Given this scenario, which option?"), Identification ("What are the four components?"), Comparison ("What's the key difference between X and Y?").
- **Distractors:** Plausible but clearly wrong to someone who learned the content. Not trick questions.

### Module Quizzes (end of each module)
- **Count:** 5-10 questions depending on module size.
- **Coverage:** All lessons in that module.
- **Spaced retrieval:** For M3 and beyond, include 1-2 questions from PREVIOUS modules. This is the single most effective way to implement spaced repetition in a self-paced program.
- **Rationales:** Each question should have a brief rationale for the correct answer (shown after submission).

---

## J. Remotion Handoff

After producing the lesson script, the Remotion compositions can be specced:

1. The lesson script contains `> **[REMOTION: ...]**` callouts — these are the handoff points
2. The Composition Summary table lists all compositions needed
3. To create detailed Remotion specs, invoke the `elevenlabs-remotion` skill with the completed lesson script
4. The Remotion spec skill uses the template at `10 - Remotion/LESSON_SPEC_TEMPLATE.md`
5. Screen recordings are NOT Remotion — they're captured separately in production
6. Note which compositions might reuse existing components vs. need new ones

---

## K. Quality Checklist

Run through this checklist before delivering the lesson script. Verify each item and flag any issues:

1. [ ] Learning objectives use observable action verbs (configure, build, test, compare — NOT understand, learn, know)
2. [ ] All nine Gagne events are present (hook, objectives, recall, content, guidance, practice, feedback, assessment, bridge)
3. [ ] VO narrative connects to what the learner has already done (prior recall)
4. [ ] Every concept has visible proof (diagram, screenshot spec, screen recording cue, or before/after)
5. [ ] Text companion mirrors video section structure exactly (same sections, same order, same headings)
6. [ ] Callout types are used correctly (Course Connection = future refs, Key Insight = tips, Good to Know = optional, Warning = pitfalls)
7. [ ] Module/lesson references use correct 8-module numbering and landing page titles
8. [ ] Product names are correct (ElevenAgents, ElevenCreative, ElevenAPI)
9. [ ] Credential name is correct ("ElevenLabs Certified ElevenAgent Builder")
10. [ ] Voice-first-plus framing (lead with voice, acknowledge chat/messaging)
11. [ ] Scaffolding matches module position (M1-M2 = max guidance, M7-M8 = minimal)
12. [ ] No marketing language in instructional content
13. [ ] Comprehension check questions are MC (A/B/C/D) if present
14. [ ] Practice section exists if the skeleton entry includes one
15. [ ] Next lesson card points to the correct next lesson
16. [ ] File naming follows convention: `M{module}-L{lesson}_v{version}_Title.md`
17. [ ] Action Mapping filter applied: no "nice to know" content in the main flow

---

## L. Reference Files

Read or reference these during authoring:

### Always read
- `01 - Certification Program/Planning & Reference/Lesson_Skeleton_Draft.md` — the source entry for the lesson being authored
- `01 - Certification Program/University_Module_Structure_Draft.md` — module-level context

### Read as needed
- `06 - Content Library/Docs & Guides/LLMS-Full ElevenLabsDevDocs.txt` — technical accuracy verification (search by topic)
- `01 - Certification Program/Design_System_Spec.md` — Remotion visual design system
- `01 - Certification Program/Lessons/Module 1/L1.2-text-companion-mockup.html` — text companion visual reference
- `10 - Remotion/LESSON_SPEC_TEMPLATE.md` — Remotion spec template for handoff

### Exemplar scripts (format reference)
- `01 - Certification Program/Experiments/v3-compact/M1-L1_v4_Welcome_to_ElevenLabs.md` — concept lesson exemplar
- `01 - Certification Program/Experiments/v3-compact/M1-L2_v1_The_Agents_Platform.md` — walkthrough lesson exemplar
- `01 - Certification Program/Experiments/v3-compact/M1-L1_v4_VO.md` — clean VO script exemplar

### Related skills
- `eu-project-context` — naming conventions, locked decisions, module structure
- `elevenlabs-brand` — brand guidelines for any ElevenLabs content
- `elevenagents-pmm` — product positioning alignment
- `elevenlabs-remotion` — Remotion spec creation (handoff target, not invoked by this skill)

---

## M. Workflow Summary

When this skill is invoked:

**Step 1: Read the skeleton.** Look up the specified lesson in the Lesson Skeleton Draft. Extract all fields.

**Step 2: Gather context.** Read the module structure for the parent module. Identify previous and next lessons. Determine what the learner has already built/learned.

**Step 3: Determine scaffolding.** Module number -> scaffolding level (M1-M2 max, M3-M4 medium, M5-M6 goal-oriented, M7-M8 minimal).

**Step 4: Select lesson pattern.** Based on the Type field (Concept, Walkthrough, Hands-On, or hybrid). Apply the matching structural pattern from Section C.

**Step 5: Author the lesson script.** Follow the output format from Section D. Weave in Gagne's events and Merrill's principles. Apply the Action Mapping filter. Include production cues, text companion content, practice exercises, and comprehension checks.

**Step 6: Extract clean VO.** Strip production directions, output VO-only file with word count.

**Step 7: Run quality checklist.** Verify all 17 items from Section K. Flag any issues.

**Step 8: Offer handoff.** Ask if Jake wants to proceed with Remotion spec creation via the `elevenlabs-remotion` skill.
