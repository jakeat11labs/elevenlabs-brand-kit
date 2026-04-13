---
name: eleven-branded-pptx
description: "Create ElevenLabs-branded PowerPoint presentations, pitch decks, and slide decks. Use this skill whenever the user wants to make a presentation that follows ElevenLabs brand guidelines — whether from scratch or by customizing the included template. Trigger on: 'ElevenLabs presentation', 'branded deck', 'branded pptx', 'brand slide deck', 'ElevenLabs slides', 'make a presentation for ElevenLabs', 'pitch deck for ElevenLabs', or any request to create slides that should look like ElevenLabs. Also trigger when editing or customizing the ElevenLabs PowerPoint template. If the user mentions both 'slides' or 'deck' and 'ElevenLabs' or 'brand', use this skill."
---

# ElevenLabs Branded Presentations

Create PowerPoint decks that follow ElevenLabs brand guidelines. This skill provides the design tokens, asset mappings, and brand rules specific to presentations — it works alongside the `/pptx` skill, which provides the actual tooling (PptxGenJS, unpack/pack scripts, QA workflow).

**Before you start:** The `/pptx` skill provides the tools to build and edit .pptx files. This skill provides the *what* (brand rules); `/pptx` provides the *how* (file manipulation).

---

## Workflow

### Default: Edit the Included Template

The plugin ships a 30-slide branded template that is fully self-contained — it already embeds 54 media files including gradient backgrounds, voice orbs, brand icons, logos, and Chladni patterns. You do not need to inject external brand assets when using this template.

**Template location:** Find the template using this priority:

1. Check `~/.elevenlabs-kit/config.json` — if `assetLocation` is `"central"`, look in the `centralPath` directory
2. Otherwise check `public/brand-assets/` in the current project
3. Look in the plugin's own `brand-assets/` directory
4. The file is named `elevenlabs-presentation-template.pptx`

**Steps:**

1. Copy the template to your working directory (never edit the original)
2. Use `python scripts/thumbnail.py` to see all 30 layouts visually
3. Use `python scripts/office/unpack.py` to extract the PPTX
4. Use `python scripts/add_slide.py` to duplicate the layouts you need
5. Edit the XML to swap placeholder text, images, and content
6. Use `python scripts/clean.py` to remove unused slides and orphaned media
7. Use `python scripts/office/pack.py` to reassemble

Read the [template layout guide](references/template-guide.md) for which slide layout to use for each purpose, with exact positions and element details.

### Fallback: Create from Scratch with PptxGenJS

When the template isn't available or the user wants a fully custom structure, use PptxGenJS with the design tokens below.

```bash
npm install -g pptxgenjs
```

---

## Template Specifications

These values come from the actual template XML — use them when editing or recreating layouts.

### Slide Dimensions

**10.00" x 5.63"** — this is the template's native size (converted from Google Slides). When creating from scratch with PptxGenJS, use the standard 16:9 default (13.33" x 7.5") instead. When editing the template, preserve its native dimensions.

### Fonts in the Template

The template uses these fonts (not KMR Waldenburg):

| Role | Font | Used On |
|------|------|---------|
| Primary | **Inter** | Most slides — titles, body, subtitles |
| Emphasis | **Inter Medium** | Slide 15, 27 — bold labels |
| Alternative | **Roboto** | Slides 22, 26 — some card/pricing content |
| Alternative | **Instrument Sans** | Slide 25 — product comparison |
| Theme default | **Arial** | Slide masters fallback |

**When editing the template:** Preserve Inter as the primary font. It's already embedded.

**When creating from scratch:** Use Inter if available, or KMR Waldenburg (the official brand font from `brand-assets/fonts/`), or Arial as final fallback.

### Colors in the Template

The template uses these hex values (extracted from actual slide XML):

| Color | Hex | Used For |
|-------|-----|----------|
| CREAM | `F5F3F1` | Card/shape fills (agenda cards, section backgrounds) |
| GRAPHITE | `1E1916` | Dark shape fills (slide 26 header bar) |
| Near-black | `292929` | Title text (slide 15) |
| Dark gray | `444746` | Agenda text (slide 6) |
| Secondary gray | `666666` | Subtitle text (slide 15) |
| Muted gray | `949494` | Secondary content text (slide 26) |
| Light fill | `F0F0F0` | Secondary card fill (agenda cards) |
| Placeholder gray | `D8D8D8` | Logo placeholder boxes (slide 27) |
| White | `FFFFFF` | Slide backgrounds, text on dark |
| Black | `000000` | Text, dark fills |

**For PptxGenJS (from-scratch):** Use hex WITHOUT the `#` prefix.

### Typography Scale (Actual Template Sizes)

| Element | Size | Font | Color |
|---------|------|------|-------|
| Cover title | 52pt | Inter | White (on dark bg) |
| Slide title | 33pt | Inter | Context-dependent |
| Section label | 26pt | Inter Medium | `292929` |
| Body / description | 14pt | Inter | Inherited from master |
| Subtitle | 12pt | Inter | `666666` |
| Pricing detail | 9-13pt | Roboto | `949494` / `1E1916` |
| Logo placeholder | 8pt | Inter | `D8D8D8` |

---

## Embedded Media in the Template

The template is self-contained with 54 media files. When editing, these are already available in `ppt/media/`:

| Type | Count | Dimensions | Format | Examples |
|------|-------|-----------|--------|----------|
| Gradient backgrounds | 8 | 2048x1152 | JPG | Title slides, closings, section dividers |
| Voice orbs | ~20 | 1174x1038 | JPG | Decorative accents across slide layouts |
| Brand icons | 7 | 336x336 | PNG | Feature card illustrations |
| Small UI icons | 8 | 60-77px | PNG | Checkmarks, indicators (pricing slides) |
| Logo wordmark | 2 | 1400x180, 1248x160 | PNG | Top-left on cover/closing slides |
| Wide banners | 2 | 2048x265, 1718x222 | PNG | Decorative elements |
| Chladni patterns | 3 | 800x800 | JPG | Texture overlays |
| Screenshots/content | 3 | Various | PNG | Placeholder product images |

**To swap images:** Replace files in `ppt/media/` with same filename and similar dimensions. Update relationship files in `ppt/slides/_rels/` if adding new images.

**To add brand assets from outside the template:** Copy images from `brand-assets/` into `ppt/media/`, add relationship entries, and reference them in slide XML.

---

## Slide Layout Summary

Read [references/template-guide.md](references/template-guide.md) for full details with exact positions.

| Slides | Layout | Key Features |
|--------|--------|-------------|
| 1-4 | **Cover & transitions** | Full-bleed bg, logo top-left (0.31", 0.21"), title at bottom-left |
| 5-6 | **Agenda** | 8 rounded-rect cards in 2×4 grid, CREAM + F0F0F0 fills |
| 7-8, 10, 17-18 | **Three key points** | 3 columns (x: 0.3", 3.5", 6.8"), icons + title + description |
| 9, 11, 13, 21-22 | **Four key points** | 4 columns (x: 0.3", 2.7", 5.2", 7.6"), with variants |
| 12 | **Blank transition** | Empty divider slide |
| 14 | **Six pillars** | 2×3 grid with icons |
| 15 | **Simple text** | Title (292929) + subtitle (666666), minimal |
| 16 | **Screenshot** | Full-width product image + description text |
| 19 | **Two screenshots** | Side-by-side image comparison |
| 20 | **Nine pillars** | 3×3 grid with icons |
| 23-24 | **Two pillars** | Two-column feature/description layouts |
| 25 | **Product comparison** | Multi-column competitor matrix with Features/Strengths/Weaknesses |
| 26 | **Pricing/solution** | Dark header (GRAPHITE fill) + 4-column feature grid with checkmarks |
| 27 | **Logo grid** | 36 logo placeholders in grid (D8D8D8 fill) |
| 28-30 | **Closing** | Full-bleed hero, "Thank you", brand statement |

---

## Slide Design Patterns

### Hero Mode (dark, atmospheric)

Full-bleed gradient background with white text. The template uses embedded 2048x1152 JPG backgrounds. Logo (white wordmark) at top-left corner, title text at lower-left.

**Template example (Slide 1):**
- Background: full-bleed image at 0,0 covering 10.00" x 5.63"
- Logo: 1.46" × 0.19" at position (0.31", 0.21")
- Title: Inter 52pt white at position (0.31", 3.26"), width 8.59"

**PptxGenJS equivalent:**
```javascript
slide.background = { path: 'background.jpg' };
slide.addImage({ path: 'logo-white.png', x: 0.31, y: 0.21, w: 1.46, h: 0.19 });
slide.addText('Title Text', {
  x: 0.31, y: 3.26, w: 8.59, h: 2.02,
  fontSize: 52, fontFace: 'Inter', color: 'FFFFFF'
});
```

### Content Mode (light, clean)

White background with dark text and CREAM card shapes. Most content slides use this pattern.

**Template example (Slide 7 — Three Key Points):**
- Background: white (inherited from master)
- Title: "Three Key Points" at (0.3", 0.4"), width 7.7", height 1.2"
- Three columns of content at x: 0.3", 3.5", 6.8" — each 2.9" wide
- Each column: icon (336x336 PNG) above, title at y=2.7", description at y=3.0"

**Template example (Slide 5 — Agenda):**
- 8 rounded rectangles in 2×4 grid
- Cards: 3.0" × 1.1" with CREAM (F5F3F1) + light (F0F0F0) fills
- Left column at x=3.5", right column at x=6.6"
- Rows at y: 0.4", 1.6", 2.8", 4.0"

### Dark Feature Mode

Used for pricing/solution slides with a dark header section.

**Template example (Slide 26):**
- Dark header: rounded rect filled GRAPHITE (1E1916) at (0.1", 0.1"), 9.9" × 2.4"
- Pricing details in header: Roboto 9-13pt, color 949494
- Below: 4-column feature grid with checkmark icons (77px PNGs)

---

## Brand Rules for Decks

These are non-negotiable:

1. **Name spelling** — "ElevenLabs" (one word, capital E, capital L). Never "Eleven Labs", "elevenlabs", or "ELEVENLABS". The II symbol is only used in the logo, never written in slide text.

2. **Logo placement** — Top-left corner of title slides (0.31", 0.21" in the template). Safespace = logo height on all sides. No effects. White on dark backgrounds, black on light.

3. **No accent lines under titles** — This is a hallmark of AI-generated slides. Use whitespace or background color contrast instead.

4. **Color from imagery** — The monochrome palette handles text and shapes. Color enters through background images, voice orbs, and photography. No colored fills or colored text except WHITE on dark backgrounds.

5. **Breathe** — The template uses 0.3" minimum margins from slide edges. Keep generous spacing between content blocks.

6. **Visual on every slide** — No text-only slides. Each slide has at least one visual element: background image, icon, shape, orb, or photograph.

7. **Consistent fonts** — When editing the template, keep Inter. When building from scratch, use Inter, KMR Waldenburg, or Arial — don't mix fonts within a deck.

---

## QA Checklist

After generating the deck, verify every slide:

- [ ] "ElevenLabs" spelled correctly (one word, capital E and L)
- [ ] Logo has safespace, no effects applied
- [ ] No accent lines under titles
- [ ] Text colors: dark text on light, WHITE on dark — no random colors
- [ ] Shape fills use only CREAM (F5F3F1), white, or dark fills — no random colors
- [ ] Icons match background contrast (light icons on dark, dark icons on light)
- [ ] Font is consistent across all slides (Inter or chosen alternative)
- [ ] Title text 26pt+ for clear hierarchy above body text
- [ ] No text-only slides
- [ ] No leftover placeholder text (search for "Insert title here", "Click to add subtitle", "Place the description", "Add section title", "Place Logo Here")
- [ ] Voice orbs on dark backgrounds or tightly cropped

Then run the full `/pptx` QA workflow: `markitdown` for content check, convert to images, visual inspection via subagent.

---

## Asset Mapping (From-Scratch Only)

When creating without the template, pull assets from `brand-assets/`:

| Slide Type | Asset Source |
|-----------|-------------|
| Title / closing | `backgrounds/gradient/Background0-15.jpg` (2048x1152) |
| Section dividers | `backgrounds/general/` gradients |
| Feature icons (dark bg) | `icons/white/` (130 white icons) |
| Feature icons (light bg) | `icons/black/` (130 black icons) |
| Accent decoration | `voice-orbs/orb-{color}-on-black.jpg` (1174x1038) |
| Logo (dark bg) | `logos/Logo-white.png` |
| Logo (light bg) | `logos/Logo-black.png` |

---

## Locating Brand Assets

1. Read `~/.elevenlabs-kit/config.json` if it exists
2. If `assetLocation` is `"central"`: assets at the `centralPath`
3. If `"project-local"` or no config: assets at `public/brand-assets/`
4. If not found: run `/elevenlabs-brand-kit:asset-setup`
