# ElevenLabs Presentation Template — Layout Guide

The template contains 30 slides, 3 slide masters (with 80 total slide layouts), and 54 embedded media files. All values below are extracted from the actual template XML.

**Slide dimensions:** 10.00" x 5.63"

## Quick Reference

| Need | Slides | Layout |
|------|--------|--------|
| Opening / title | 1-4 | Cover & transition |
| Table of contents | 5-6 | Agenda (8 sections) |
| 3 features / points | 7, 8, 10, 17, 18 | Three-card with icons |
| 4 features / points | 9, 11, 13, 21, 22 | Four-card with icons |
| 6 items overview | 14 | Six pillars (2x3) |
| 9 items matrix | 20 | Nine pillars (3x3) |
| Simple statement | 15 | Text + subtitle |
| Product screenshot | 16 | Hero image + description |
| Side-by-side compare | 19 | Two screenshots |
| Two-column features | 23-24 | Two pillars with detail |
| Competitor matrix | 25 | Product comparison table |
| Pricing / solution | 26 | Dark header + feature grid |
| Partner logos | 27 | Logo grid (36 slots) |
| Closing | 28-30 | Hero + thank you |

---

## Slides 1-4: Cover & Transitions

### Slide 1 — Title Cover

Full-bleed background image covering entire slide. ElevenLabs white logo wordmark top-left. Large title at bottom-left.

| Element | Position | Size | Details |
|---------|----------|------|---------|
| Background image | 0.00", 0.00" | 10.00" x 5.63" | Full-bleed JPG (image7.jpg, 2048x1152) |
| Logo wordmark | 0.31", 0.21" | 1.46" x 0.19" | White PNG (image8.png, 1400x180) |
| Title text | 0.31", 3.26" | 8.59" x 2.02" | Inter 52pt, white |

**Placeholder text:** "Legal Training" / "x ElevenLabs"

### Slides 2-3 — Company Name Hero

Full-bleed gradient background with centered "Company name" text and ElevenLabs branding. Slide 2 includes a tall vertical image element (image12.png, 1481x2048).

| Element | Position | Size | Details |
|---------|----------|------|---------|
| Background image | full-bleed | 10.00" x 5.63" | Gradient JPG |
| Logo | 0.31", ~0.21" | ~1.46" x 0.19" | White wordmark |
| Title | varies | — | Inter 33pt, white |

### Slide 4 — Gradient Transition with Cream

Gradient background with logo and title. Uses brand colors directly:

| Element | Details |
|---------|---------|
| Colors in XML | `F5F3F1` (CREAM), `1E1916` (GRAPHITE), `000000` |
| Font | Inter 33pt |

---

## Slides 5-6: Agenda

Eight numbered sections in a 2-column, 4-row grid using rounded rectangle cards.

### Card Grid Layout

| Position | Card Location | Card Size |
|----------|--------------|-----------|
| Row 1, Left | 3.5", 0.4" | 3.0" x 1.1" |
| Row 1, Right | 6.6", 0.4" | 3.0" x 1.1" |
| Row 2, Left | 3.5", 1.6" | 3.0" x 1.1" |
| Row 2, Right | 6.6", 1.6" | 3.0" x 1.1" |
| Row 3, Left | 3.5", 2.8" | 3.0" x 1.1" |
| Row 3, Right | 6.6", 2.8" | 3.0" x 1.1" |
| Row 4, Left | 3.5", 4.0" | 3.0" x 1.1" |
| Row 4, Right | 6.6", 4.0" | 3.0" x 1.1" |

| Element | Details |
|---------|---------|
| Card shape | `roundRect` |
| Card fills | `F5F3F1` (CREAM) + `F0F0F0` (light gray) |
| Section title text boxes | 2.7" wide, inside each card |
| Section numbers | 01-08, positioned left of section titles |
| Title | "Agenda" — positioned at left side |
| Colors | Slide 5: `292929`, `F0F0F0`, `F5F3F1`. Slide 6: `444746`, `F0F0F0` |

**Placeholder text:** "Add section title" in each card.

---

## Slides 7-8, 10, 17-18: Three Key Points

Three-column layout with icon above each column, then title + description text.

### Standard Three-Column Positions (Slide 7)

| Element | Col 1 | Col 2 | Col 3 |
|---------|-------|-------|-------|
| Column x | 0.3" | 3.5" | 6.8" |
| Column width | 2.9" | 2.9" | 2.9" |
| Icon | 336x336 PNG above text | 336x336 PNG | 336x336 PNG |
| Title y | 2.7" | 2.7" | 2.7" |
| Title height | 0.3" | 0.3" | 0.3" |
| Description y | 3.0" | 3.0" | 3.0" |
| Description height | 2.0" | 2.0" | 2.0" |

| Element | Details |
|---------|---------|
| Slide title | (0.3", 0.4") — 7.7" x 1.2", "Three Key Points" |
| Subtitle | (0.3", 0.2") — 2.4" wide, "Click to add subtitle" |
| Icons | 3 embedded 336x336 PNGs (image9, image15, image21, etc.) |

**Variants:**
- **Slide 8:** Same layout but with different spacing and extra description rows
- **Slide 10:** Similar three-column, different vertical positioning
- **Slide 17:** "Three Pillars" — icons only with titles, no description text
- **Slide 18:** "Three Points" — includes 4-line description text areas

**Placeholder text:** "Insert title here" + "Place the description of the point here. Keep it clear, informative, and no longer than 3 short lines of text."

---

## Slides 9, 11, 13, 21-22: Four Key Points

Four-column layout with icons and content cards.

### Standard Four-Column Positions (Slide 9)

| Element | Col 1 | Col 2 | Col 3 | Col 4 |
|---------|-------|-------|-------|-------|
| Column x | 0.3" | 2.7" | 5.2" | 7.6" |
| Column width | 2.1" | 2.1" | 2.1" | 2.1" |
| Title y | 2.7" | 2.7" | 2.7" | 2.7" |
| Description y | 3.0" | 3.0" | 3.0" | 3.0" |

| Element | Details |
|---------|---------|
| Slide title | (0.3", 0.4") — 7.7" x 1.2" |
| Icons | 4 embedded 336x336 PNGs |

**Variants:**
- **Slide 11:** Similar four-column layout
- **Slide 13:** Four columns with different spacing
- **Slide 21:** Four key points with longer 4-line description areas
- **Slide 22:** Image+text variant — uses Roboto font, "Two lines Headline" format with images below, descriptions below images. Card fill `F5F3F1`.

---

## Slide 12: Blank Transition

Empty divider slide — just a title placeholder ("#") with no content. Use between major sections.

---

## Slide 14: Six Pillars

2×3 grid of icon + title cards.

| Row | Col 1 x | Col 2 x | Col 3 x | y |
|-----|---------|---------|---------|---|
| Top | 0.3" | 3.5" | 6.8" | 2.3" |
| Bottom | 0.3" | 3.5" | 6.8" | 4.6" |

Each cell: 2.9" wide, icon (336x336 PNG) above, title (0.4" height) below.

**Placeholder text:** "Insert title here" in each cell.

---

## Slide 15: Simple Text

Minimal slide with large title and subtitle.

| Element | Position | Size | Details |
|---------|----------|------|---------|
| Title | 0.3", 0.2" | 9.4" x 0.5" | Inter Medium 26pt, fill color `292929` |
| Subtitle | 0.3", 0.7" | 9.1" x 0.3" | Inter 12pt, fill color `666666` |

**Placeholder text:** "Simple slide with a title" / "And a subtitle for longer description"

---

## Slide 16: Screenshot / Hero Image

Product screenshot with description text.

| Element | Position | Size | Details |
|---------|----------|------|---------|
| Title | top area | — | "Slide with Screenshot" |
| Screenshot | center | large area | Placeholder product image (PNG) |
| Description | below | — | Multi-line body text about product features |

---

## Slide 19: Two Screenshots

Side-by-side image comparison.

| Element | Position | Size |
|---------|----------|------|
| Title | top | "Two Screenshots" |
| Left image | left half | Placeholder screenshot |
| Right image | right half | Placeholder screenshot |

---

## Slide 20: Nine Pillars

3×3 grid with icons and titles.

| Row | Col 1 x | Col 2 x | Col 3 x |
|-----|---------|---------|---------|
| Top | 0.3" | 3.5" | 6.8" |
| Middle | 0.3" | 3.5" | 6.8" |
| Bottom | 0.3" | 3.5" | 6.8" |

Each cell has an icon (336x336) and title. Keep text very short — cells crowd quickly at this density.

---

## Slides 23-24: Two Pillars with Description

Two-column feature layouts with icon + title + description per column.

### Slide 23

| Element | Position | Details |
|---------|----------|---------|
| Title | top | "Insert title here" |
| Left column | left half | Icon + title + description |
| Right column | right half | Icon + title + description |

### Slide 24

Similar layout: "Two Pillars with Description" — two icons with corresponding content.

---

## Slide 25: Product Comparison

Multi-column competitor comparison matrix.

| Element | Details |
|---------|---------|
| Font | **Instrument Sans** (unique to this slide) |
| Colors | `F5F3F1`, `CFE2F3` (light blue), `D9EAD3` (light green), `E992D5` (pink), `E9E9E9`, `FFFCFA` |
| Structure | 4 competitor columns × 3 row categories (Features, Strengths, Weaknesses) |
| Text size | 9pt |

---

## Slide 26: Pricing / Our Solution

Dark header with pricing details, followed by 4-column feature checklist.

| Element | Position | Size | Details |
|---------|----------|------|---------|
| Dark header bar | 0.1", 0.1" | 9.9" x 2.4" | `roundRect` filled GRAPHITE (`1E1916`) |
| Pricing items | within header | 2.0" x 0.6" each | Roboto, `949494` text |
| Column headers | 0.3", 2.9" | 2.0" wide each | Category labels at x: 0.3", 2.7", 5.1", 7.5" |
| Feature rows | below headers | — | Feature name + checkmark icons (77px PNGs) |

**Pricing layout in header:**

| Item | Position |
|------|----------|
| "Conversational AI / $0.08/minute" | 5.6", 0.5" |
| "Speech transcription / $0.22/hour" | 7.8", 0.5" |
| "Audio quality / 128 kbps, 44.1kHz" | 5.6", 1.3" |
| "API formats / 16kHz PCM, uLaw" | 7.8", 1.3" |

---

## Slide 27: Logo Grid

36 logo placeholder slots arranged in a grid.

| Element | Details |
|---------|---------|
| Placeholder fill | `D8D8D8` (light gray), also `F5F3F1`, `FFFFFF` |
| Font | Inter, Inter Medium, 8pt |
| Text | "Place Logo Here" in each slot |

---

## Slides 28-30: Closing

### Slide 28 — Hero Closing
Full-bleed background image (image69.jpg, 2048x1152) with tagline "The Trusted Voice Agent Platform" at 20pt.

### Slide 29 — Thank You
Full-bleed background with logo wordmark and "Thank you" in Inter 52pt.

### Slide 30 — Brand Statement
Minimal closing slide (smallest XML file). Use for final brand statement or leave as clean end card.

---

## Working with the Template

### Duplicating Slides

```bash
python scripts/add_slide.py unpacked/ --duplicate <slide-number>
```

### Swapping Content

After unpacking, each slide is an XML file in `ppt/slides/`:
- `<a:t>` tags: text content
- `<a:rPr>` tags: text formatting (preserve these)
- `<a:latin typeface="Inter"/>`: font references
- `<a:srgbClr val="F5F3F1"/>`: color values
- `<p:pic>` elements: image references → files in `ppt/media/`
- `<a:off x="" y=""/>` and `<a:ext cx="" cy=""/>`: positions in EMU (914400 EMU = 1 inch)

### Replacing Images

1. Add new image to `ppt/media/` (e.g., `image_new.jpg`)
2. Add relationship in `ppt/slides/_rels/slideN.xml.rels`:
   ```xml
   <Relationship Id="rIdNew" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/image" Target="../media/image_new.jpg"/>
   ```
3. Reference `rIdNew` in the slide XML's `<a:blip r:embed="rIdNew"/>`

### Removing Unused Slides

```bash
python scripts/clean.py unpacked/
```

### Preserving Brand Styling

- Don't modify `ppt/slideMasters/` or `ppt/slideLayouts/`
- Keep font references (`Inter`, `Arial`) intact
- Preserve color values — use only the colors documented above
