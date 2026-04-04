# WorkRamp Text Companion Guide

This reference defines how to generate a text companion document for any lesson in the ElevenLabs Academy. The output should be structured so Jake can copy-paste block by block into the WorkRamp Guide editor.

## Table of Contents

1. Output Format
2. WorkRamp Content Blocks (full reference)
3. WorkRamp Knowledge Check Blocks (full reference)
4. Text Companion Template
5. Block Usage Patterns
6. Visual Style Rules
7. Section Mapping (Video ↔ Arcade ↔ Text)

---

## 1. Output Format

Each text companion is a markdown document with **WorkRamp block tags** indicating which editor block to use. Tags look like this:

```
[WR: Text]
[WR: Image]
[WR: Callout — Tip]
[WR: Accordion]
[WR: Embed]
[WR: Divider]
```

Jake reads the tag, creates that block type in the WorkRamp editor, and pastes the content. This removes guesswork.

---

## 2. WorkRamp Content Blocks (Full Reference)

These are the content blocks available in the WorkRamp Guide editor (Content tab). Every block is available for any lesson — use the right tool for the job.

### Text
Rich text editor supporting bold, italic, links, inline images, and HTML. The workhorse block — use for paragraphs, step sequences, numbered lists, and any flowing content.

### Callout
Four visual styles for breaking up text and adding context:
- **Important** — Use for Course Connection callouts (links content to other modules, "You'll configure this in Module 5"). Graphite-feel border.
- **Info** — Use for Good to Know callouts (helpful context, not essential). Blue border.
- **Tip** — Use for Key Insight callouts (important takeaways, tips, best practices). Green border.
- **Warning** — Use for Warning callouts (things to watch out for, common mistakes). Amber border.

### Attach File
PDFs preview inline; docs/spreadsheets get download link. Use for supplemental reference material, cheat sheets, templates.

### Video
Upload video with options: enable download, full watch required, disable speed adjust. Supports thumbnails and closed captions (AI-generated or uploaded). Recommended specs: 1920x1080, no black border. Thumbnail: 1280x780. This is where the Remotion/Premiere lesson video goes.

### Image
Images, GIFs, and short videos via drag-drop. Use for annotated screenshots, architecture diagrams, reference cards, and short looping GIFs (5-10 seconds) that show multi-click flows.

### Audio
For recorded calls, voiceovers, podcast-style content. Could be useful for audio examples in voice-focused lessons.

### Divider
Line breaks with customizable thickness, pattern, and color. Use between major sections of the text companion.

### Flip Card
Front: question or term. Back: answer or definition. Lighter than graded questions. Use for quick vocabulary checks, concept reinforcement, or "did you catch this?" moments within lessons. Not a formal assessment.

### Accordion
Expandable content blocks. Can be marked as required. Use for deeper detail that keeps the main flow clean: sidebar breakdowns, "what does each setting do" expansions, advanced options, optional deep-dives.

### Record Video
In-browser recording (no screen share). Typically not used — we produce video externally.

### Record with Zoom
Launches Zoom, supports screen share, auto-saves to Guide. Could be used for live workshop recordings.

### Embed
Embeds external web pages directly in the Guide via iframe. This is where **Arcade interactive demos** go. Also works with: Google Drive, YouTube, Box, LinkedIn Learning, password-protected sites. Could embed ElevenLabs docs, API reference, or sandbox environments.

**Arcade embed format:**
```html
<iframe src="https://app.arcade.software/share/{flowId}?embed="
  title="[Demo Title]"
  frameborder="0"
  loading="lazy"
  webkitallowfullscreen mozallowfullscreen allowfullscreen
  allow="clipboard-write"
  style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; color-scheme: light;">
</iframe>
```

### AI Assist
Open prompting to create content from scratch. Built into WorkRamp — Jake can use this as a starting point and refine.

---

## 3. WorkRamp Knowledge Check Blocks (Full Reference)

These are on the "Knowledge Checks" tab in the editor. Available for any lesson where a comprehension check or formal assessment makes sense.

### Text Answer
Free-form text response. Can be AI auto-graded with reviewer notes. Use when you want learners to articulate understanding in their own words.

### Multiple Choice
Standard A/B/C/D (or more) options. Our default for module quizzes and comprehension checks. Always use this for formal assessments.

### Hot Spot
Click on a specific area of an image to answer. Could be useful for "identify this part of the UI" type questions.

### Survey
Collect opinions or feedback. Not graded. Useful for end-of-module learner feedback, beta testing, "how helpful was this lesson?"

### Matching
Match items from two columns. Could work for mapping terminology to definitions, or connecting features to their purposes.

### File Upload
Learner uploads a file. Could be used in the certification exam for submitting agent configurations or screenshots.

### To-Do Checklist
Interactive checklist learners work through. Good for practice sections where learners need to complete a series of hands-on tasks.

### Video Recording
Learner records video response. Could be used for portfolio-style submissions in advanced tracks.

### Zoom Recording
Learner records via Zoom. Unlikely for V1 but available.

### Voice Recording
Learner records audio response. Interesting potential for voice-related lessons — "record yourself configuring a voice."

### E-Signature
Collect a digital signature. Available for compliance or agreement workflows.

---

## 4. Text Companion Template

Use this structure for every text companion. Adapt sections based on content type (video, Arcade, or both).

```markdown
# [Lesson Title]

[WR: Text]
**Module [X] — [Module Name]** | Lesson [X.Y]
[Type badge: Concept / Walkthrough / Hands-On] | ~[X] min read

[One-liner description of what this lesson covers]

**Learning Objectives**
By the end of this lesson, you will be able to:
1. [Objective 1]
2. [Objective 2]
3. [Objective 3]

---

[WR: Video]  ← if this is a video lesson
[Upload: M{XX}_L{YY}_v{Z}.mp4]
[Thumbnail: 1280x780]
[Captions: AI-generated]

---

[WR: Embed]  ← if this is an Arcade lesson
[Arcade embed code — iframe src from Arcade share page]
[Title: "[Descriptive demo title]"]

---

[WR: Divider]

## Section 1: [Section Name]

[WR: Text]
[Section introduction — 2-3 sentences setting context]

[WR: Image]
[Screenshot: description of what's shown]
[Caption: what the learner should notice]
[Annotation: amber (#F59E0B) highlight boxes on key areas]

[WR: Text]
### Step-by-step: [Process Name]
1. **[Step title]** — [Description]. [Optional inline screenshot reference]
2. **[Step title]** — [Description].
3. **[Step title]** — [Description].

[WR: Callout — Tip]
**Key Insight:** [Important takeaway from this section]

[WR: Accordion]
**[Expandable topic — e.g., "What does each setting do?"]**
[Detailed content that would clutter the main flow]

---

[WR: Divider]

## Section 2: [Section Name]
[Repeat pattern: Text → Image → Steps → Callouts → Accordions]

---

[WR: Divider]

## Practice

[WR: Text]
**Hands-On Exercises**
Try these on your own:

☐ [Exercise 1 — specific, actionable]
☐ [Exercise 2 — builds on the lesson]
☐ [Exercise 3 — stretch goal]

[WR: Callout — Info]
**Good to Know:** Don't worry about getting everything perfect. You'll refine these skills throughout the certification.

---

[WR: Divider]

## Up Next

[WR: Text]
**Lesson [X.Y+1]: [Next Lesson Title]**
[One sentence teaser of what's coming next]
```

---

## 5. Block Usage Patterns

### When to use each callout type

| Callout Type | WorkRamp Style | Use When... | Example |
|---|---|---|---|
| Course Connection | Important | Linking to future/past modules | "You'll configure advanced prompts in Module 3" |
| Key Insight | Tip | Sharing an important takeaway or best practice | "Always test your agent with edge cases before deploying" |
| Good to Know | Info | Adding helpful but non-essential context | "The API also supports batch processing, which we'll cover later" |
| Warning | Warning | Flagging gotchas or common mistakes | "Changing the voice model mid-conversation will reset the session" |

### When to use Accordion vs inline text

**Use Accordion when:**
- Content is > 3 paragraphs of detail on a subtopic
- It's a "reference table" (e.g., all settings and what they do)
- Advanced users want depth, beginners can skip it
- It's a sidebar explanation that interrupts the main flow

**Use inline text when:**
- Content is core to understanding the lesson
- It's 1-2 paragraphs
- Every learner needs to read it

### When to use Flip Card

- Vocabulary/terminology checks within a lesson
- Quick "do you remember?" moments after a concept section
- Pairing a feature name with its purpose
- NOT for formal assessment (use Multiple Choice for that)

### Screenshots and Images

- Use multiple small screenshots distributed throughout (not one big image)
- Consistent annotation style: amber (#F59E0B) for highlight boxes and callout markers
- Always include a caption below explaining what the learner is seeing
- For multi-click flows where statics fail, use a short GIF (5-10 seconds, looping)
- Screengrab images from Remotion scenes come from: `10 - Remotion/screengrabs/M{XX}L{YY}/`
- Screenshots from screen recordings are captured during Premiere assembly

---

## 6. Visual Style Rules

- **Annotation color:** Amber (#F59E0B) for all highlight boxes, callout markers, and pointer arrows on screenshots
- **Font in WorkRamp:** Use WorkRamp's default — don't try to force KMR Waldenburg (that's for Remotion/docs only)
- **Monochrome approach:** Keep the text companion visually clean. Use brand colors for callout borders but let WorkRamp handle the styling
- **Image resolution:** Screenshots at 1920x1080 (from screen recordings or Remotion screengrabs). WorkRamp handles responsive sizing.
- **GIF specs:** 5-10 seconds, looping, captured from screen recordings. Keep file size under 5MB.

---

## 7. Section Mapping

The text companion mirrors whatever visual medium the lesson uses:

**For Video lessons:** Text sections map 1:1 to video sections. If the video has Section 1 (Three Pillars), Section 2 (Foundation Diagram), Section 3 (Timeline) — the text has the same three sections in the same order. Remotion screengrabs become reference card images.

**For Arcade lessons:** Text sections map 1:1 to Arcade segments. Each segment of the recording plan becomes a section in the text. The Arcade demo itself embeds via the Embed block, and the text provides the annotated detail around it.

**For Both (Video + Arcade):** The video typically handles the concept/overview content and the Arcade handles the hands-on UI content. The text weaves both together: video screengrabs for concept sections, Arcade embed for the walkthrough sections.

A learner should be able to pause the video (or step away from the Arcade) at any point, scroll to the matching section in the text, and find annotated stills or written steps of exactly what they're looking at.
