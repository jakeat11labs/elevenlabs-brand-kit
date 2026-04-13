---
name: branded-pptx
description: "Create ElevenLabs-branded PowerPoint presentations, pitch decks, and slide decks. Use this skill whenever the user wants to make a presentation that follows ElevenLabs brand guidelines — whether from scratch or by customizing the included template. Trigger on: 'ElevenLabs presentation', 'branded deck', 'branded pptx', 'brand slide deck', 'ElevenLabs slides', 'make a presentation for ElevenLabs', 'pitch deck for ElevenLabs', or any request to create slides that should look like ElevenLabs. Also trigger when editing or customizing the ElevenLabs PowerPoint template. If the user mentions both 'slides' or 'deck' and 'ElevenLabs' or 'brand', use this skill."
---

# ElevenLabs Branded Presentations

Create PowerPoint decks that follow ElevenLabs brand guidelines. This standalone skill bundles the official 30-slide ElevenLabs template with all embedded media (backgrounds, voice orbs, icons, logos). It works alongside the `/pptx` skill for actual file manipulation.

**Requires:** The `/pptx` skill for PptxGenJS, unpack/pack scripts, and QA workflows.

---

## Template

This skill ships with the ElevenLabs template at `assets/[Template] ElevenLabs Presentation _ Update.pptx`. It contains 30 professionally designed slides, 3 slide masters (80 layouts), and 54 embedded media files — gradient backgrounds, voice orbs, brand icons, logos, and Chladni patterns. No external asset downloads needed.

---

## Workflow

### Default: Edit the Template

1. Copy `assets/[Template] ElevenLabs Presentation _ Update.pptx` to your working directory
2. Use `python scripts/thumbnail.py` to see all 30 layouts
3. Use `python scripts/office/unpack.py` to extract
4. Use `python scripts/add_slide.py` to duplicate the layouts you need
5. Edit XML to swap placeholder text, images, and content
6. Use `python scripts/clean.py` to remove unused slides
7. Use `python scripts/office/pack.py` to reassemble

Read [references/template-guide.md](references/template-guide.md) for which layout to use and exact positioning data.

### Fallback: Create from Scratch

When the user wants a fully custom structure, use PptxGenJS (`npm install -g pptxgenjs`) with the design tokens below. You can also unpack the template to extract its embedded images for use in from-scratch decks.

---

## Template Specifications

### Slide Dimensions

**10.00" x 5.63"** (template native). When creating from scratch with PptxGenJS, use standard 16:9 (13.33" x 7.5").

### Fonts

| Role | Font | Used On |
|------|------|---------|
| Primary | **Inter** | Most slides — titles, body, subtitles |
| Emphasis | **Inter Medium** | Bold labels |
| Theme fallback | **Arial** | Slide masters |

When creating from scratch: use Inter if available, or KMR Waldenburg (official ElevenLabs brand font), or Arial.

### Colors (from template XML)

PptxGenJS hex values (no `#` prefix):

| Color | Hex | Use |
|-------|-----|-----|
| CREAM | `F5F3F1` | Card/shape fills |
| GRAPHITE | `1E1916` | Dark shape fills (pricing headers) |
| Near-black | `292929` | Title text |
| Secondary gray | `666666` | Subtitle text |
| Muted gray | `949494` | Secondary content |
| Light fill | `F0F0F0` | Secondary card fill |
| Placeholder | `D8D8D8` | Logo placeholder boxes |
| White | `FFFFFF` | Backgrounds, text on dark |
| Black | `000000` | Text, dark fills |

### Typography Scale

| Element | Size | Font | Color |
|---------|------|------|-------|
| Cover title | 52pt | Inter | White |
| Slide title | 33pt | Inter | Context-dependent |
| Section label | 26pt | Inter Medium | `292929` |
| Body text | 14pt | Inter | Inherited |
| Subtitle | 12pt | Inter | `666666` |
| Detail text | 9-13pt | Roboto | `949494` / `1E1916` |

---

## Embedded Media

The template bundles these assets in `ppt/media/`:

| Type | Count | Dimensions | Format |
|------|-------|-----------|--------|
| Gradient backgrounds | 8 | 2048x1152 | JPG |
| Voice orbs | ~20 | 1174x1038 | JPG |
| Brand icons | 7 | 336x336 | PNG |
| Small UI icons | 8 | 60-77px | PNG |
| Logo wordmark | 2 | 1400x180, 1248x160 | PNG |
| Chladni patterns | 3 | 800x800 | JPG |

To use these in a from-scratch deck, unpack the template and copy images from `ppt/media/`.

---

## Slide Layout Summary

| Slides | Layout | Key Features |
|--------|--------|-------------|
| 1-4 | **Cover & transitions** | Full-bleed bg, logo at (0.31", 0.21"), title bottom-left |
| 5-6 | **Agenda** | 8 rounded-rect cards, 2x4 grid, CREAM + F0F0F0 fills |
| 7-8, 10, 17-18 | **Three key points** | 3 columns (x: 0.3", 3.5", 6.8"), each 2.9" wide |
| 9, 11, 13, 21-22 | **Four key points** | 4 columns (x: 0.3", 2.7", 5.2", 7.6"), each 2.1" wide |
| 12 | **Blank transition** | Empty divider |
| 14 | **Six pillars** | 2x3 grid with icons |
| 15 | **Simple text** | Title (292929) + subtitle (666666) |
| 16 | **Screenshot** | Full-width product image |
| 19 | **Two screenshots** | Side-by-side comparison |
| 20 | **Nine pillars** | 3x3 grid with icons |
| 23-24 | **Two pillars** | Two-column feature/description |
| 25 | **Product comparison** | Multi-column competitor matrix |
| 26 | **Pricing/solution** | Dark GRAPHITE header + 4-col feature grid |
| 27 | **Logo grid** | 36 placeholder slots |
| 28-30 | **Closing** | Full-bleed hero + thank you |

---

## Slide Design Patterns

### Hero Mode (Slides 1-4, 28-30)

Full-bleed gradient background, white text, logo top-left.

```javascript
slide.background = { path: 'background.jpg' };
slide.addImage({ path: 'logo-white.png', x: 0.31, y: 0.21, w: 1.46, h: 0.19 });
slide.addText('Title', { x: 0.31, y: 3.26, w: 8.59, h: 2.02, fontSize: 52, fontFace: 'Inter', color: 'FFFFFF' });
```

### Content Mode (Most slides)

White background, dark text, CREAM card shapes.

```javascript
slide.background = { fill: 'FFFFFF' };
slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
  x: 3.5, y: 0.4, w: 3.0, h: 1.1,
  fill: { color: 'F5F3F1' }, rectRadius: 0.1
});
slide.addText('Section Title', { x: 3.7, y: 0.9, w: 2.7, h: 0.5, fontSize: 14, fontFace: 'Inter', color: '292929' });
```

### Dark Feature Mode (Slide 26)

Dark header bar + content grid below.

```javascript
slide.addShape(pptx.shapes.ROUNDED_RECTANGLE, {
  x: 0.1, y: 0.1, w: 9.9, h: 2.4,
  fill: { color: '1E1916' }, rectRadius: 0.1
});
```

---

## Brand Rules (Non-Negotiable)

1. **"ElevenLabs"** — one word, capital E and L. The II is logo-only, never in text.
2. **Logo** — top-left on title slides (0.31", 0.21"). Safespace = logo height. No effects.
3. **No accent lines under titles** — hallmark of AI-generated slides. Use whitespace.
4. **Color from imagery** — monochrome text/shapes, color from background images only.
5. **Breathe** — 0.3" minimum margins, generous spacing.
6. **Visual on every slide** — no text-only slides.
7. **Consistent fonts** — don't mix fonts within a deck.

---

## QA Checklist

- [ ] "ElevenLabs" spelled correctly (one word, capital E, capital L)
- [ ] Logo has safespace, no effects
- [ ] No accent lines under titles
- [ ] Text: dark on light, WHITE on dark — no random colors
- [ ] Shape fills: CREAM, white, or dark only
- [ ] Icons match background contrast
- [ ] Fonts consistent across all slides
- [ ] No leftover placeholder text ("Insert title here", "Click to add subtitle", "Place the description", "Add section title", "Place Logo Here")
- [ ] Visual element on every slide

Run `/pptx` QA: `markitdown` for content, convert to images, visual inspection via subagent.

---

## Additional Brand Assets

For more backgrounds, icons, voice orbs, and fonts beyond what's embedded in the template, install the full ElevenLabs Brand Kit plugin:

```
/plugin marketplace add jakerains/elevenlabs-brand-kit
/plugin install elevenlabs-brand-kit@elevenlabs-brand-kit
```

Then run `/elevenlabs-brand-kit:asset-setup` to download ~75MB of brand assets including 52+ backgrounds, 400+ icons, 8 voice orbs, logos, and KMR Waldenburg fonts.
