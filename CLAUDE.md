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
- `source/team_cast.png` - reference portrait of the recurring Scrum team that appears in all photos (left to right: Leen, Stef, Arnaud, Sara, Mohammed, Pieter the Scrum Master). A realistic mix for a Belgian IT team. Pass it as the first `--ref` when generating new images so the same people return.

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
- `#empirisme` — The principle only: T/I/A cycle diagram (`.empir-cycle`) with the course definitions, and the PXL photo-app worked example (`EMPIR_EXAMPLE[]`, predict-then-reveal stepper). Ends with a bridge to `#scrum-waarden`. Prefix: `empir-`.
- `#scrum-waarden` — Introduces the Campi team (`img/team_cast.jpg` with names and roles), the five Scrum values with the course's one-line definitions, then five dilemmas from Sprint 1 (`WAARDE_DILEMMAS[]`, one per value; each option either `right` or with `feedback`). Solving a dilemma highlights its value. Ends with how the values enable empiricism and a bridge to `#sprint-events`. Placed here because the course text treats the values right after empiricism. Prefix: `waarde-`. The pillar is called "Aanpassing" on the page ("Adaptatie" only as the Scrum Guide synonym).
- `#sprint-events` — Overview strip of the 5 events (Sprint as container, timeboxes), then the day-by-day 2-week Sprint walk-through. Steps are events, not days (`sprintSteps()`): weekends are optional, day 12 is split into Daily/Review/Retro sub-steps, and an end card plus ghost "Sprint 2" cell show the loop. Refinement is an optional layer after the Sprint (`role="switch"` toggle, `REFINEMENT_OVERRIDES` via `getActiveDay()`); toggling keeps progress. Ends with a recap and a bridge to `#empirisme-events`.
- `#empirisme-events` — After the Sprint: students pick an event and classify its three activities as T/I/A (`EMPIR_EVENTS[]`, order shuffled per event via `EMPIR_ORDERS`; wrong answers get a hint from `EMPIR_HINTS` keyed by chosen + correct pillar). Correct answers fill a 3×5 keyword table. Then a transfer quiz with 5 scenarios (`EMPIR_SCENARIOS[]`, prefix `scenario-`) about team tools and agreements, deliberately not overlapping the scanner activities. Key words in the `EMPIR_EVENTS[]` explanation texts are wrapped in `<mark>` and get an animated highlighter stripe in the pillar color.
- `#poker-planning` — Planning Poker for the "Campi" campus app. A 6-step procedure strip, then 6 User Stories (`POKER_STORIES[]`) in teaching order: consensus, small spread with a team convention, big spread with a round 2 (`revote`), discussion, too big (split), not Ready (?-cards). The student plays along as sixth team member: picks a card from their hand face down, all cards flip together (3D flip on `.poker-card`, 5:7 cards with a black/gold PXL back), then answers one question per story (`question.options`, each with a wrong-answer `feedback`; the "average" option is always a trap). Ends with a summary (4 rules + the estimated backlog) and a bridge to `#sprint-planning`. Prefix: `poker-`.
- `#sprint-planning` — The Planning of Campi Sprint 1. Intro with `img/sprint_planning.jpg`, four facts, then 3 steps: a good User Story and acceptance criterion (`PLAN_QS.story`), the Sprint Doel (`PLAN_QS.goal`), and filling the Sprint Backlog from the Product Backlog in priority order within a capacity of 17 storypunten (`PLAN_ITEMS[]` with the Planning Poker estimates; not-Ready and too-big items are refused; correct set `PLAN_CORRECT`). Shows the Sprint Backlog in three parts (why, what, how), a Product Backlog vs Sprint Backlog table, and a bridge to `#daily-scrum`. The chosen Sprint Backlog is exactly what the Review shows. Prefix: `plan-`.
- `#daily-scrum` — The Daily Scrum (Daily Standup) on day 8 of Campi Sprint 1. Intro with `img/daily_standup_burndown.jpg`, four facts and the three questions, then 3 steps: sort Mohammed's update into gisteren/vandaag/obstakel (`DAILY_SORT[]`, hints in `DAILY_SORT_HINT`), recognise three anti-patterns (`DAILY_WRONG[]`: technical discussion, reporting to the Scrum Master, "alles goed" while the board says otherwise), and read the Scrum Board plus an inline SVG burndown chart (`DAILY_READ[]`; the team is behind, the fix is finishing work in Doing together). The board tasks match the Planning's Sprint Backlog, and fit the Review's outcome: by helping each other the team finishes US-02, while US-05 stays untested. Bridge to `#sprint-review`. Prefix: `daily-`.
- `#sprint-review` — The Review of Campi Sprint 1 (product, not process). Intro with `img/sprint_review.jpg` and named stakeholders (Rudy, PXL Catering; Nadia, studentenadministratie; Kevin, student), four facts, then 3 steps: pick which stories may be shown (`REVIEW_STORIES[]`, only Done ones), what happens with stakeholder feedback (`REVIEW_QS[]`, it goes to the Product Backlog via the Product Owner), and classify stakeholders as intern/extern/interface (`REVIEW_HOLDERS[]`, course categories). Ends with the updated Product Backlog and a bridge to `#retrospectives`. Prefix: `review-`.
- `#retrospectives` — The Retrospective of Campi Sprint 1, facilitated by Pieter, in 4 steps, then the ending of the whole page. Prefix: `retro-`. See pattern notes below.

**Image pipeline:** High-res source images (`source/*.png`, 7–8MB) are resized using Python/Pillow and saved as JPEGs in `img/`. Always convert RGBA to RGB before saving as JPEG (`img.convert('RGB')`). Always resize before committing to keep the repo lightweight. Typical widths: 600px for scene photos, 900px for board/whiteboard images (e.g. retro boards).

**Retrospective section pattern** (`#retrospectives`):
- Step 1 (`RETRO_PREV[]`): were the previous action points carried out? The student answers per blue post-it, using evidence from earlier sections (the Sprint dailies, the not-Ready poker story).
- Step 2 (`RETRO_NOTES[]`): the Start/Stop/Continue board (`img/StartStopContinue.jpg`). Each note has `col`, `who` (a team member), `text`, `top`/`left` (% of the board) and `rot`; notes with `sort: true` are placed by the student via Start/Stop/Continue buttons. Post-its are absolutely positioned on the board; below 620px they are hidden and the same content is shown as a list per column (`#retro-list`).
- Step 3 (`RETRO_ACTIONS[]`): the student picks exactly 2 of 5 candidate actions. The two `good` ones are SMART and have a `short` text for the board's action boxes; the others carry `feedback` (vague, unrealistic, not time-bound).
- Step 4 (`RETRO_FQ[]`, `RETRO_FORMATS`): which format fits which situation (Mad/Sad/Glad, Zeilboot with rocks, Start/Stop/Continue), with a preview of the board after the right answer.
- The ending is always visible: the 4 Retro rules, a loop back to `#sprint-events` (Sprint 2 starts) and a recap of the whole page.
- Pedagogical note: the format is only a tool. The emphasis is on starting with the previous actions, and on a *small* number of SMART actions that will actually be executed. Unexecuted actions come back as frustration in the next Retro.

**Reference project:** `../PMTheBasics/projectmanagement-basis.html` — sibling repo with 5 interactive sections using the same patterns (triangle/diamond diagrams, PSS builder, SMART challenges, lifecycle stepper). Use as reference for new sections.

## Development
No build commands. Open `agile.html` directly in a browser to test. The deploy workflow uploads the entire repo root as a static site.
