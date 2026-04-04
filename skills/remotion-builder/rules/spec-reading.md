# Reading Spec Files

Spec files are your blueprint. They tell you exactly what to build. Here's how to parse them.

## Spec File Location

`10 - Remotion/M{XX}L{YY}v{Z}-spec.md`

## Spec Structure

```markdown
# M{XX}L{YY}v{Z} — [Lesson Title]

**Module:** [number] — [module name]
**Lesson:** [number] — [lesson name]
**Version:** [number]
**Duration:** ~[total] seconds ([scene count] scenes)
**VO Pace:** ~145 words/min, conversational

## Video Summary
[2-3 sentences about the lesson]

## Scene Plan
### Scene 1 — Title Card
[scene block]
...
### Scene N — Outro
[scene block]

## Scene Flow Summary
[table of all scenes]
```

## Scene Block Format

Each scene provides everything you need:

```markdown
### Scene [N] — [Scene Name]
| | |
|---|---|
| **Layout** | [layout type] |
| **Component** | [component name or blank] |
| **Duration** | [seconds] |
| **VO Beat** | [the voiceover line] |

**Content:**
- **Section Label:** `VALUE`
- **Headline:** `VALUE`
- **[more fields per layout...]**

**Notes:** [special behavior, active items, etc.]
```

## Mapping Spec → Code

### Layout → Component

| Spec Layout | Look For Existing | Create If Needed |
|-------------|-------------------|------------------|
| Split Title Card | `WaveformScene`, `OriginStory`, `OutroScene` | Rarely — well-covered |
| Three-Column Cards | `ThreePillarsScene`, `ChannelStrip`, `TextRevealScene` | Only if 3-card structure differs |
| Three-Step Flow | `PlatformEvolution` | If step structure differs |
| Split List | `TimelineProgression` | If list structure differs |
| Centered Hero | `WelcomeCard` | Rarely needed |
| Connected Rows | `FoundationDiagram` | If row structure differs |
| Hero Text Panels | `LearnBuildProve` | If panel structure differs |
| Split + Feature Grid | `PillarDetailScene` | Rarely — very flexible |
| Two-Column Cards | `TwoColumnCards` (layout) | Has its own component |
| Four-Grid Cards | `FourGridCards` (layout) | Has its own component |
| Single Feature Card | `SingleFeatureCard` (layout) | Has its own component |
| Quote Card | `QuoteCard` (layout) | Has its own component |
| Side-by-Side Comparison | `SideBySideComparison` (layout) | Has its own component |
| Stat Number | `StatNumber` (layout) | Has its own component |
| Stat Row | `StatRow` (layout) | Has its own component |
| Numbered Steps | `NumberedSteps` (layout) | Has its own component |
| Checklist | `Checklist` (layout) | Has its own component |
| Code Display | `CodeDisplay` (layout) | Has its own component |
| Full-Bleed Placeholder | `FullBleedPlaceholder` (layout) | Has its own component |
| Split Quote | `SplitQuote` (layout) | Has its own component |

### Component Field → Behavior

- **If the Component field names an existing component:** Use it directly
- **If the Component field is blank:** The agent (you) decides which existing component or layout to use, or creates a new one
- **If the Component field names something new:** Create it following established patterns

### Content Fields → Props

Map the Content section to component props:

```markdown
**Content:**
- **Section Label:** `PRODUCT ARCHITECTURE`
- **Headline:** `Three Pillars, One Foundation`
```

Becomes:

```tsx
<ThreePillarsScene
  sectionLabel="PRODUCT ARCHITECTURE"
  headline="Three Pillars, One Foundation"
/>
```

### Notes → Special Behavior

The Notes field tells you about:
- **Active items:** "Module 1 is active" → set `isActive` or highlight styling
- **Badge display:** "Cert Focus badge on card 2" → render the badge component
- **Image references:** "Uses Cover0.jpg" → pass the right cover image
- **Timing:** "Caption appears late (delay 55 frames)" → add delay to that element
- **Split ratios:** "55/45 split" → adjust width percentages
- **Missing watermark:** "No II watermark" → omit `<IIWatermark />`

## Duration → durationInFrames

```tsx
// Spec says: Duration: 8 seconds
// Code:
durationInFrames={sec(8)}  // where sec = (s) => s * fps
```

## Scene Flow Summary → Verification

The table at the bottom of the spec is your checklist. After building, verify:
1. Same number of scenes
2. Same order
3. Same layouts
4. Same durations
5. Same total duration

## Handling Reused Components

When a spec uses the same component multiple times with different content (e.g., PillarDetailScene for Creative, Agents, API), each gets:

- Its own `TransitionSeries.Sequence` in the combined composition (with different props)
- Its own `<Composition>` registration in Root.tsx (with different defaultProps and unique IDs)
- They share the same schema and component code

## Handling Full-Bleed Placeholders

When a spec includes `FullBleedPlaceholder` scenes, these are spots where screen recordings or b-roll footage will be inserted in post-production. Build the placeholder component — don't try to insert actual footage.
