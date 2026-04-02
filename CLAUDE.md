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
- `source/daily_standup_2ndday.png` - an image of a scrum team having a daily standup at the 2nd day of a sprint
- `source/daily_standup_lastday.png` - an image of a scrum team having a daily standup at the last day of a sprint
- `source/daily_standup_burndown.png` - an image of a scrum team having a daily standup while looking at a burndown chart in the middle of a sprint
- `source/sprint_planning.png` - an image of a scrum team having a sprint planning
- `source/sprint_review.png` - an image of a scrum team having a sprint review
- `source/sprint_retro.png` - an image of a scrum team having a sprint retrospective
- `source/team_relaxing.png` - an image of a scrum team during the weekend
- `source/team_relaxing_clicked.png` - an image of a scrum team during the weekend, to show when hovered above the other image
- `source/refinement session.png` - an image of a scrum team having a refinement session
- `source/poker planning.md` - a few examples of scenarios of how to do a poke planning where a scrum team uses poker cards
- `source/MadSadGlad.png` - an image of an empty Mad Sad Glad retrospective board
- `source/StartStopContinue.png` - an image of an empty Start Stop Continue retrospective board
- `source/Sailboat.png` - an image of an empty Sailboat retrospective board
- `source/empirisme.png` - an image showing an image explaining empirisme

## Requirements
- Single-page scroll layout (all sections on one page, nav links are anchor scrolls)
- Clean, modern design
- Max-width: 1100px (optimized for 1920x1080 student screens)

## Hosting — GitHub Pages
The site is published via GitHub Actions to GitHub Pages.

- **Workflow**: `.github/workflows/deploy.yml` — triggers on every push to `main`
- **Source setting**: GitHub Pages → Source must be set to **GitHub Actions** (in repo Settings → Pages)
- No build step — the entire repo root is uploaded as the Pages artifact
- **Gotcha:** Pushing workflow files requires the `workflow` OAuth scope. If rejected, add the file via GitHub web UI instead.

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
  --sprint-blue: #5b7fa6;      /* Sprint Planning event color */
  --refinement: #8b5cf6;       /* Refinement session color (purple) */
}
```

**Typography:** Raleway (600–900) for headings via Google Fonts CDN; Arial/system-ui for body.

**Nav:** Black bg (`--primary`), PXL logo (`pxl-logo-64.png`, 34×34px), course name in gold (e.g. "Projectmanagement"), page subtitle in `#bbb` (e.g. "Basisbegrippen"). Sticky, `z-index: 100`. Nav links are hidden behind a **hamburger menu** (`#nav-hamburger` button + `#nav-dropdown` div). The dropdown opens/closes via `navToggle()` / `navClose()` and closes on outside click or Escape. Add new nav items as `<a href="#section" onclick="navClose()">` inside `#nav-dropdown`.

**Footer:** Black bg, centered PXL logo + "Hogeschool PXL", full line: "Hogeschool PXL • Elfde-Liniestraat 24 • B-3500 HASSELT • www.pxl.be".

**Section badges:** Gold bg, dark text, `font-weight: 800`, `text-transform: uppercase`, small `letter-spacing`.

**Callout blocks:** `border-left: 4px solid var(--gold)`, gold-tinted background, italic text.

**Max-width:** 1100px centered, optimized for 1920×1080 student screens.

## Language
All page content is in **Dutch** (Nederlands).
Do NOT use em dashes (—, `&mdash;`, `\u2014`) in any user-visible text. Use periods, colons, commas, or parentheses instead.

## Architecture
Single-file HTML pages with inline `<style>` and `<script>` — no build step, no framework.

**Pattern:** Each interactive section follows the same structure:
1. **Data array** — JS array of objects defining content (e.g., `SPRINT_DAYS[]`, `POKER_STORIES[]`)
2. **Init IIFE** — builds DOM elements on page load
3. **Click handler** — updates active state, visited set, renders detail panel via `innerHTML`
4. **Next/Prev navigation** — sequential stepping through the data
5. **State** — global vars: `sectionActive`, `sectionVisited = new Set()`
6. **Summary card** — appears when all items visited (`.visible` class toggle)

**CSS naming:** Each section uses a unique prefix (e.g., `sprint-`, `poker-`) for all classes and IDs.

**Current sections in `agile.html`:**
- `#empirisme` — Two-part section: (A) "Scrum Event Scanner" 5×3 matrix where students discover T/I/A in each Scrum Event, (B) "Scenario Sorter" quiz with 12 practice scenarios. Prefixes: `empir-`, `scenario-`.
- `#sprint-events` — Interactive 2-week Sprint timeline with day-by-day Scrum events. Has a toggle switch between official Scrum Guide view and a practice view with Refinement sessions (uses `REFINEMENT_OVERRIDES` overlay pattern via `getActiveDay()` helper).
- `#poker-planning` — Planning Poker simulation with 8 User Stories for "Campi" campus app, each demonstrating a different estimation scenario (consensus, big spread, too big, etc.).
- `#retrospectives` — Three retrospective formats (Start/Stop/Continue, Mad/Sad/Glad, Zeilboot) shown as animated post-it replays. Prefix: `retro-`. See pattern notes below.

**Image pipeline:** High-res source images (`source/*.png`, 7–8MB) are resized using Python/Pillow and saved as JPEGs in `img/`. Always convert RGBA to RGB before saving as JPEG (`img.convert('RGB')`). Always resize before committing to keep the repo lightweight. Typical widths: 600px for scene photos, 900px for board/whiteboard images (e.g. retro boards).

**Retrospective section pattern** (`#retrospectives`):
- Data lives in `RETRO_FORMATS[]` — each entry has `id`, `name`, `img`, `rules[]`, and `postits[]`.
- Each postit has `text`, `top`/`left` (% strings for absolute positioning over the board image), `rot` (degrees), and optionally `blue: true` for action-point post-its.
- The board image is rendered inside `.retro-board-wrap` as a 100%-wide `<img>`. Post-its are `position: absolute` children, animated with `opacity` + `transform: scale` transition.
- Clicking either the "Volgende post-it" button or the board image itself advances to the next post-it (`onclick="retroNext()"`). The board wrapper has `cursor: pointer`.
- Post-its have `pointer-events: none` so clicks pass through them to the wrapper.
- Yellow post-its = team observations. Blue post-its = action points (rendered last, positioned in the "Actiepunten" area of the board image).
- Pedagogical note: the formats shown are examples only — many formats exist. The emphasis is on a *small* number of actions that will actually be executed, rather than a long list that gets ignored. Unexecuted actions from a previous retro are themselves a source of frustration in the next one.

**Reference project:** `../PMTheBasics/projectmanagement-basis.html` — sibling repo with 5 interactive sections using the same patterns (triangle/diamond diagrams, PSS builder, SMART challenges, lifecycle stepper). Use as reference for new sections.

## Development
No build commands. Open `agile.html` directly in a browser to test. The deploy workflow uploads the entire repo root as a static site.
