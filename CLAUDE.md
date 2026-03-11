# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project
Interactive learning webapp that teaches students Agile (and Scrum). Part of the PXL Hogeschool "Projectmanagement" course series.

## Source materials (in `source/` folder)
- `source/007 Agile Projectmanagement.md` — contains the lesson content and structure
- `source/007 Agile Projectmanagement.pdf` — exact the same text content as `007 Agile Projectmanagement.md` but with images
- `source/2025_10_huisstijlhandboek.pdf` — PXL corporate identity / huisstijlhandboek
- `source/1314_logo_pxl_bol_witrand.png` — original PXL logo (high-res)
- `source/daily_standup_early.png` - an image of a scrum team having a daily standup in the beginning of a sprint
- `source/daily_standup_late.png` - an image of a scrum team having a daily standup nearly at the end of a sprint
- `source/daily_standup_burndown.png` - an image of a scrum team having a daily standup while looking at a burndown chart in the middle of a sprint
- `source/sprint_planning.png` - an image of a scrum team having a sprint planning
- `source/sprint_review.png` - an image of a scrum team having a sprint review
- `source/sprint_retro.png` - an image of a scrum team having a sprint retrospective
- `source/team_relaxing.png` - an image of a scrum team during the weekend

## Requirements
- Single-page scroll layout (all sections on one page, nav links are anchor scrolls)
- Clean, modern design
- Max-width: 1100px (optimized for 1920x1080 student screens)

## Hosting — GitHub Pages
The site is published via GitHub Actions to GitHub Pages.

- **Workflow**: `.github/workflows/deploy.yml` — triggers on every push to `main`
- **Source setting**: GitHub Pages → Source must be set to **GitHub Actions** (in repo Settings → Pages)
- No build step — the entire repo root is uploaded as the Pages artifact

## Styling — PXL Hogeschool huisstijl
All pages follow the PXL corporate identity:

**CSS design tokens (copy exactly into each new page):**
```css
:root {
  --primary: #030203;           /* PXL zwart — nav bg, headings */
  --gold:    #AE9A64;           /* PXL goud — badges, accents, borders */
  --gold-bg: rgba(174,154,100,0.12);
  --accent:  #e63946;           /* red — warnings, critical, "laag" state */
  --green:   #2a9d8f;           /* "hoog" / positive state */
  --bg:      #f8f7f5;
  --card:    #ffffff;
  --border:  #e0ddd6;
  --text:    #1a1a1a;
  --muted:   #666;
}
```

**Typography:** Raleway (600–900) for headings via Google Fonts CDN; Arial/system-ui for body.

**Nav:** Black bg (`--primary`), PXL logo (`pxl-logo-64.png`, 34×34px), course name in gold (e.g. "Projectmanagement"), page subtitle in `#bbb` (e.g. "Basisbegrippen"). Sticky, `z-index: 100`.

**Footer:** Black bg, centered PXL logo + "Hogeschool PXL", full line: "Hogeschool PXL • Elfde-Liniestraat 24 • B-3500 HASSELT • www.pxl.be".

**Section badges:** Gold bg, dark text, `font-weight: 800`, `text-transform: uppercase`, small `letter-spacing`.

**Callout blocks:** `border-left: 4px solid var(--gold)`, gold-tinted background, italic text.

**Max-width:** 1100px centered, optimized for 1920×1080 student screens.

## Language
All page content is in **Dutch** (Nederlands).

## Architecture
Single-file HTML pages with inline `<style>` and `<script>` — no build step, no framework.

**Pattern:** Each interactive section follows the same structure:
1. **Data array** — JS array of objects defining content (e.g., `SPRINT_DAYS[]`)
2. **Init IIFE** — builds DOM elements on page load
3. **Click handler** — updates active state, visited set, renders detail panel via `innerHTML`
4. **Next/Prev navigation** — sequential stepping through the data
5. **State** — global vars: `sectionActive`, `sectionVisited = new Set()`
6. **Summary card** — appears when all items visited (`.visible` class toggle)

**CSS naming:** Each section uses a unique prefix (e.g., `sprint-`) for all classes and IDs.

**Reference project:** `../PMTheBasics/projectmanagement-basis.html` — sibling repo with 5 interactive sections using the same patterns (triangle/diamond diagrams, PSS builder, SMART challenges, lifecycle stepper). Use as reference for new sections.

## Development
No build commands. Open `agile.html` directly in a browser to test. The deploy workflow uploads the entire repo root as a static site.
