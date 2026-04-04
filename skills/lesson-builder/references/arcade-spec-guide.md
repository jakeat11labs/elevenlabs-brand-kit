# Arcade Recording Spec Guide

This reference defines how to generate an Arcade recording spec — the detailed plan Jake follows when recording an interactive demo using the Arcade Mac desktop app (or Chrome extension).

## Table of Contents

1. What an Arcade Spec Is
2. Arcade Capabilities (Quick Reference)
3. Spec Template
4. Recording Best Practices
5. Example: Sidebar Navigation Spec

---

## 1. What an Arcade Spec Is

An Arcade recording spec is a segment-by-segment plan for a lesson's interactive demo. Each segment maps to a section of the text companion and (if the lesson also has video) a section of the VO script. The spec tells Jake:

- What screen to start on
- What to click, in what order
- Where to place hotspots (clickable dots that advance the demo)
- Where to place callouts (floating text explanations)
- Where to use spotlight areas (dim everything except the focus region)
- When to insert chapter steps (section dividers)
- What the screen should look like at the end of each segment

The spec is NOT the final Arcade — it's the recording plan. Jake records following the spec, then polishes in Arcade's editor (trimming, repositioning hotspots, adding callouts, adjusting timing).

---

## 2. Arcade Capabilities (Quick Reference)

Reference the full docs at `06 - Content Library/Docs & Guides/Arcade llms-full.txt` for deep detail. Here's what's available:

### Recording Methods
- **Desktop app (Mac/Windows)** — Records any screen: browser, terminal, desktop apps, multi-tab flows. Supports Interactive mode (screenshots + video) and Video-only mode. Can record from connected mobile devices.
- **Chrome extension** — Best for browser-only web apps. Captures clicks as screenshots + hotspots, scrolls/typing as video. Supports Auto Pan and Zoom (Chrome only).
- **Media upload** — Import MP4s, screenshots, PDFs, YouTube links. AI Smart Split breaks video into steps.
- **Figma plugin** — Import PNG frames from Figma.

### Interactive Elements
- **Hotspots** — Pulsating clickable dots. Created automatically at each click during recording. Can set destination (next step, any step, or external URL). Can add text, set pointer position, toggle voiceover generation.
- **Callouts** — Floating text boxes with arrow pointers. Support markdown (bold, italic, links, lists). For guidance/tips without requiring a click. Can toggle between hotspot and callout styles.
- **Spotlight Areas** — Dim everything except a defined rectangular region. Available on hotspots and callouts. Supports square, rounded, or circle shapes. Customizable border and overlay opacity. Use for complex UI where you need to draw attention to a specific area.
- **Chapters** — Section dividers with title, subtitle, and up to 4 navigation buttons. Can link to different steps or external URLs. Customizable themes (light/dark/custom). Great for branching (Growth plan) or section breaks.
- **Forms** — Collect information (text, email, choice, hidden fields). Responses stored in Insights. Can gate progression. Not needed for V1 academy content but available.
- **Embed steps** — Insert external HTML (HubSpot forms, Calendly, etc.) directly in the Arcade.
- **Menu Button** — Persistent nav button in bottom-left. Expandable with custom items linking to steps or URLs.

### Design & Branding
- **Themes** — Reusable design presets (fonts via Google Fonts, brand colors for hotspots/callouts, wrapper style, background, cursor, watermark). Can apply at workspace, folder, or Arcade level.
- **Wrapper** — Browser frame around the Arcade (light/dark/none).
- **Background** — Decorative area around the Arcade (presets, custom upload, or none).
- **Cursor** — Standard, enlarged, or mobile-style tap cursor.
- **Watermark** — Remove (Pro) or replace with custom logo (Growth).
- **Player Controls** — Toggle visibility of: volume, captions, translation, copy link, full screen, back, next, replay.

### Audio
- **Voiceover recording** — Record audio/video directly in the editor. Auto-generates captions.
- **Audio upload** — Upload MP3 per step or as background music. Auto-generates captions.
- **Synthetic voice** — AI voiceover via ElevenLabs integration. Choose from multiple voices and languages. Toggle per hotspot/callout.
- **Background music** — 5 built-in tracks or upload MP3. Audio ducking auto-lowers music when voice plays. Recommended volume: ~20%.
- **Captions** — Auto-generated for all audio/video. Editable. Can be shown/hidden by viewer.

### AI Features (Avery)
- **Auto-copy generation** — Avery writes hotspot text during recording based on HTML, visual analysis, metadata, and URL context. Tones: Promotional, Educational, or custom.
- **Script View** — Document-style view of all Arcade text. Edit all copy at once. AI can improve, fix, change tone, or shorten.
- **Smart Split** — AI splits uploaded video into individual steps.
- **Translation** — AI translates Arcade into 49+ languages.

### Sharing & Embedding
- **Embed code** — HTML iframe or React component. Display options: inline (recommended), popout tab, popout modal.
- **Step-by-Step Guide export** — Converts Arcade to static scroll-through guide (images + hotspot text as headers). Export as PDF, copy-paste to docs, or share via link. Useful as a starting point for text companions.
- **GIF/Video export** — Export as GIF or MP4 for social, email, presentations. Presets: Email (GIF), Social (MP4), Presentation (MP4), Custom.
- **Custom Links** — Multiple share links per Arcade with separate settings and tracking.

### Analytics
- **Flow Insights** — Per-Arcade performance: views, play rate, completion rate, CTA clicks, average play time, step-by-step drop-off.
- **Branch engagement** — Per-button click rates and drop-off for chapter branching.
- **Form submissions** — Stored in Insights, exportable as CSV.
- **Audience Reveal** — Company identification via Clearbit (Growth plan).
- **Integrations** — Amplitude, Mixpanel, PostHog, Google Analytics, Google Tag Manager.

---

## 3. Spec Template

Use this format for every Arcade recording spec:

```markdown
# Arcade Recording Spec: M{XX}L{YY} — [Lesson Title]

**Module:** [X] — [Module Name]
**Lesson:** [X.Y] — [Lesson Title]
**Content Type:** Text + Arcade [or Text + Video + Arcade]
**Estimated Steps:** [total across all segments]
**Recording Method:** Desktop App — Interactive mode
**Screen Size:** Standard desktop (recommended: use Arcade's resize dropdown)

---

## Recording Overview

[2-3 sentences: what this Arcade demo covers, what the learner will click through, and what they'll understand by the end.]

---

## Pre-Recording Setup

- [ ] Log in to ElevenLabs at [URL]
- [ ] Navigate to [starting screen/state]
- [ ] [Any setup needed — create a test agent, prepare data, etc.]
- [ ] Open Arcade desktop app, select Interactive mode
- [ ] Set screen size via Resize dropdown (standard desktop)
- [ ] [Optional] Enable Avery with Educational tone for starter copy

---

## Segment Plan

### Segment 1: [Segment Name]
| | |
|---|---|
| **Maps to** | Text companion Section [X]: [Section Name] |
| **Starting screen** | [URL or UI state — e.g., "ElevenLabs dashboard, Agents tab selected"] |
| **Estimated steps** | [number] |

**Click Path:**
1. [Click/action] → [What happens — e.g., "Agents sidebar panel opens"]
2. [Click/action] → [What happens]
3. [Click/action] → [What happens]

**Hotspot Placements:**
- Step 1: [Location on screen — e.g., "On the 'Agents' tab in the left sidebar"]
- Step 3: [Location — e.g., "On the 'Create Agent' button, top right"]

**Callout Text:**
- Step 1: "[Explanatory text — e.g., 'This is the Agents hub — every agent you build lives here.']"
- Step 2: "[Text — e.g., 'Each card shows the agent name, status, and last edited date.']"

**Spotlight Areas:**
- Step 2: [What to spotlight — e.g., "Spotlight the agent cards grid, dim the sidebar and header"]

**End State:** [What the screen looks like — e.g., "Agent creation modal is open, showing the name and description fields"]

---

### Chapter: [Section Divider Title]  ← optional, use between major segments
| | |
|---|---|
| **Theme** | [Light / Dark / Custom] |
| **Title** | [Chapter title] |
| **Subtitle** | [Chapter subtitle] |
| **Buttons** | [Button 1 label → destination], [Button 2 label → destination] |

---

### Segment 2: [Segment Name]
[Repeat the segment format above]

---

## Post-Recording Checklist

- [ ] Review all steps in Arcade editor
- [ ] Trim any video segments that are too long
- [ ] Reposition hotspots for clarity
- [ ] Add/edit callout text (replace Avery's auto-copy with our copy from the spec)
- [ ] Add spotlight areas where specified
- [ ] Insert chapter steps where specified
- [ ] Set Arcade design: [wrapper style], [background], [branding]
- [ ] Preview on desktop and mobile
- [ ] Publish and grab embed code for WorkRamp
- [ ] [Optional] Export Step-by-Step Guide as starting material for text companion
```

---

## 4. Recording Best Practices

**Capture resolution:** Record at a consistent screen size using Arcade's Resize dropdown. Arcade captures at 2x resolution, so use a high-DPI display (your laptop screen) rather than an external monitor for best quality.

**Keep segments focused:** Each segment should cover one distinct task or area. A segment of 5-10 steps is ideal. More than 15 steps in a segment means it should probably be split.

**Click deliberately:** In Interactive mode, every click creates a screenshot + hotspot. Pause briefly between clicks so Arcade captures clean states. Avoid clicking on things you don't want as steps.

**Use video for typing/scrolling:** Arcade auto-records video when it detects scroll, type, or drag actions. This is fine — you can trim video segments in the editor afterward.

**Plan your callout copy before recording:** Even though Avery can auto-generate copy, having the text ready means less post-editing. The spec's callout text is what should end up in the final Arcade.

**Spotlight for complex UIs:** When the ElevenLabs dashboard has a lot going on (sidebar, header, main content, modals), use spotlight areas to dim everything except what you're talking about. This is much more effective than just pointing at something in a busy UI.

**Chapters for natural breaks:** If the Arcade covers multiple distinct workflows (e.g., "creating an agent" then "testing an agent"), insert a chapter step between them. This gives the learner a pause point and makes the navigation progress bar more useful.

**Step-by-Step export as text companion starter:** After publishing the Arcade, export the Step-by-Step Guide (Share → Step-by-Step). This gives you images + hotspot text in a scroll-through format. Use this as raw material for the text companion — the screenshots become your annotated images, and the hotspot text becomes your captions.

---

## 5. Example: Sidebar Navigation Spec

Here's a condensed example of what a segment looks like for navigating the ElevenLabs sidebar:

```markdown
### Segment 1: Platform Sidebar Tour
| | |
|---|---|
| **Maps to** | Text companion Section 1: The Sidebar |
| **Starting screen** | elevenlabs.io dashboard, logged in, Conversational AI tab |
| **Estimated steps** | 7 |

**Click Path:**
1. Click "Conversational AI" in left sidebar → Agents list view opens
2. Hover over agent cards area → [no click, just showing the layout]
3. Click "Phone Numbers" in sidebar → Phone numbers management view
4. Click "Knowledge Base" in sidebar → Knowledge base list view
5. Click "Tools" in sidebar → Tools configuration view
6. Click "Analytics" in sidebar → Analytics dashboard
7. Click back to "Conversational AI" → Return to agents list

**Hotspot Placements:**
- Step 1: On "Conversational AI" sidebar item
- Step 3: On "Phone Numbers" sidebar item
- Step 4: On "Knowledge Base" sidebar item
- Step 5: On "Tools" sidebar item
- Step 6: On "Analytics" sidebar item
- Step 7: On "Conversational AI" sidebar item

**Callout Text:**
- Step 1: "This is your home base. Every agent you build and manage lives here."
- Step 3: "Phone Numbers is where you provision and assign numbers for voice agents."
- Step 4: "Knowledge Base lets you upload documents your agent can reference during conversations."
- Step 5: "Tools connect your agent to external APIs and actions."
- Step 6: "Analytics shows conversation logs, performance metrics, and cost tracking."

**Spotlight Areas:**
- Step 1: Spotlight the left sidebar navigation, dim the main content area
- Step 3-6: Spotlight the main content area, dim the sidebar (since we're showing what each section contains)

**End State:** Agents list view, showing all sidebar sections have been visited
```
