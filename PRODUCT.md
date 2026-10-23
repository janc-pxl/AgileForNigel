# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Students in the professional bachelor Toegepaste Informatica at Hogeschool PXL (PXL-Digital), following the course Projectmanagement (OLOD 42TIN1250). They are learning Agile and Scrum, often while working in their own student project teams.

The page is used in four situations, all of which matter:

- **Projected in class:** a lecturer walks through it on a projector or shared screen.
- **In class on their own laptop:** students click through the interactions themselves (typically 1920×1080 screens).
- **Self-study at home:** preparing for or revising after a lesson, and before the exam.
- **On phones:** a significant share of students open it on a phone.

The lecturers (Lowie Vangaal, Jan Castermans) are a secondary audience: they teach with the page and maintain it.

## Product Purpose

An interactive learning webapp that turns the lesson "007 Agile Projectmanagement" into things students do rather than read: discovering where Transparantie, Inspectie and Adaptatie live in each Scrum Event, stepping through a two-week Sprint day by day, and playing through Planning Poker scenarios.

Success is twofold, weighted equally: students understand the theory well enough to answer exam questions, and they can apply Scrum (run the events, estimate with story points) in their own project team.

## Positioning

It is not a generic Scrum explainer. It is built directly on this course's own lesson text and terminology, in Dutch, set in the PXL context, with scenarios grounded in a recognisable student-team world (the fictional "Campi" campus app, photos of a Scrum team through a Sprint). Each concept is learned through a hands-on simulation tied to the course material, so the page doubles as an in-class teaching tool and a self-study companion.

## Operating Context

- Accompanies the lesson text in `source/007 Agile Projectmanagement.md` (same content as the PDF). The page is a companion to that text, not a replacement.
- Lecturers project it live and step through sections in front of the group; students then repeat the interactions on their own devices.
- Students revisit it on their own for exam preparation.
- Students are simultaneously working in project teams where they apply Scrum.

## Capabilities and Constraints

- **Current sections in `agile.html`:**
  - `#empirisme`: the principle only: the T/I/A cycle with the course definitions and the PXL photo-app worked example (predict the pillar per step).
  - `#sprint-events`: overview of the 5 events (Sprint as container), then an interactive two-week Sprint walk-through, with Refinement as an optional practice layer afterwards.
  - `#empirisme-events`: after the Sprint, students find T/I/A in each Scrum Event (fills a 5×3 overview table), then a short transfer quiz with 5 scenarios about team tools and agreements.
  - `#poker-planning`: Planning Poker simulation with 8 User Stories for "Campi", each showing a different estimation situation.
- **Direction:** the page will grow to cover more of the course. Candidate chapters from the lesson text include Wat is Agile, incrementeel vs. iteratief, Scrum Waarden, Product Backlog, de drie Rollen, Scrum Artefacten, hulpmiddelen tijdens de Sprint, and tijdsregistratie. Which ones, and in what order, is not decided yet.
- **Technical:** static single-file HTML with inline CSS and JS. No framework, no build step. Deployed to GitHub Pages from the repo root via GitHub Actions on push.
- **Layout:** single-page scroll; the nav links are anchor scrolls. Content max-width is 1100px.
- **Images:** high-res sources in `source/` are resized to roughly 600px-wide JPEGs in `img/` before committing.
- **Terminology:** official Scrum terms are kept as in the lesson text (Sprint, Product Backlog, Daily Scrum, Increment, Product Owner, Scrum Master, storypunten, and so on).

## Brand Commitments

- **Hogeschool PXL corporate identity is binding.** The huisstijlhandboek is at `source/2025_10_huisstijlhandboek.pdf`; the logo is `pxl-logo-64.png` (original: `source/1314_logo_pxl_bol_witrand.png`). Nav and footer follow the PXL pattern documented in `CLAUDE.md` (black nav and footer, logo, "Hogeschool PXL • Elfde-Liniestraat 24 • B-3500 HASSELT • www.pxl.be").
- **Language:** all user-visible content is in Dutch.
- **No em dashes** in user-visible text. Use periods, colons, commas, or parentheses instead.
- **Voice:** direct and student-facing ("je"), explanatory, with a light touch (the page contains an easter egg).

## Evidence on Hand

- Lesson text: `source/007 Agile Projectmanagement.md` and `.pdf`.
- Planning Poker scenarios: `source/poker planning.md`.
- Photos of a Scrum team through a Sprint (sources in `source/`, web versions in `img/`): sprint planning, daily standup (early, mid-sprint with burndown, late), refinement session, sprint review, sprint retrospective, team relaxing at the weekend (plus a hover variant).
- No learner data, usage analytics, or student testimonials exist. Do not invent them.

## Product Principles

1. **Learn by doing.** Every concept gets an interaction that makes the student decide, discover or step through something, not just read.
2. **Stay true to the course.** Content and terminology follow the lesson text and the Scrum Guide; the practice view clearly separates real-world habits (such as Refinement sessions) from the official guide.
3. **Works on every screen it is used on.** A projector at the back of a classroom, a student laptop, and a phone are all first-class.
4. **Theory and practice together.** Each section should help with both the exam and the student's own project team.
5. **Simple to maintain.** One static file and plain patterns, so lecturers can extend it without tooling.

## Accessibility & Inclusion

Legibility from a distance matters because the page is projected in class. No formal standard has been set; WCAG 2.2 AA is the sensible default unless the lecturers decide otherwise.
