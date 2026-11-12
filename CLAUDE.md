# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What it is

Ryvath Mattey's personal portfolio page, `index.html`.

It is a single self-contained static page: all CSS is in one `<style>` block in `<head>`, and all JS is inline (a tiny script in `<head>` plus one at the end of `<body>`). There is no build step, package manager, linter, or test suite. The only external dependency is Google Fonts (JetBrains Mono, Inter).

`resume.pdf` is a copy of `~/Documents/RyvathMatteyResumes/RESUME_RMattey.pdf`; recopy it when the resume changes. `design/terminal-mockup.html` is the design-tool mockup the page was built from; it is kept locally and deliberately not committed, since GitHub Pages would publish it. It uses that tool's own template format, so treat it as a visual spec only and never copy its markup.

Preview: `open index.html`, or `python3 -m http.server` from this folder and visit `http://localhost:8000`.

## Design direction

The page is a single terminal / editor window centered on a near-black page: a header bar (window dots, `ryvath@athens:~/<file>` path, icon links for resume, GitHub, LinkedIn, email), an EXPLORER sidebar listing the six sections as files (`about.md`, `chartwise/`, `projects/`, `experience.log`, `stack.toml`, `contact.sh`), a main panel showing one section at a time, and a status bar with the current path and position (`2/6`). Each section opens with a shell command that types itself out.

- **Colors** (defined as `:root` variables; add new ones there rather than inline hex): page `#070A0F`, window `#0A0E14`, text `#E5E9F0`, secondary `#B4BCC8`, muted `#8B95A1`, borders `#1E2630`, accent mint `#7FEEA0`, amber `#FFBD2E` for dates. Dark only. Keep text contrast at 4.5:1 or better.
- **Fonts:** JetBrains Mono for UI, headings, and anything "terminal"; Inter for paragraphs.
- **Real content only.** Every terminal element must show something true: file names are real sections, the path and position reflect the open section, the failure-drills log lists drills that were actually run with their measured results. No joke stats, fake output, or placeholder data.
- **Motion stays small** (typing command, staggered fade-up, card lift, arrow nudge, blinking cursor, cursor glow on fine pointers) and every animation is off under `prefers-reduced-motion`.

## How the page works

- **Sections** are `<section class="pane" id="…">` inside `<main>`. Sidebar items, About shortcut cards, and the prev/next pager are plain `<a href="#id">` links. The script turns clicks into `history.pushState` + render, so the back button and direct links work; left/right arrow keys step through sections (ignored in form fields or with modifiers). The `SECTIONS` array in the script and the `html.js[data-section="…"]` CSS rules must list the same ids, so adding, removing, or reordering a section means updating both, the sidebar, each section's pager links, and the nav count metadata.
- **No-JS fallback:** the head script adds `js` to `<html>`; only then are inactive sections hidden. Without JS all sections stack in order, every pipeline stage description shows (with `.nojs-only` titles), and `.js-only` controls (stage buttons, copy button) are hidden.
- **Chartwise pipeline:** each `.stage` button's `aria-controls` names a `.stage-detail`; the open one has `.is-open`.
- **Mobile (< 760px):** the sidebar stacks above the content, the window grows to fit, and nothing scrolls inside a box. Touch targets are at least 44px. Put any new mobile override in that media query.
- **Accessibility:** real `<a>`/`<button>` elements, `aria-current="page"` on the active sidebar item, `aria-label` on icon-only links, visible `:focus-visible` rings. Pager and card navigation moves focus to the new section's heading (`tabindex="-1"`).

## Content accuracy

Project text quotes specific metrics (test counts, throughput, latencies, accuracy). Chartwise, batchserve, and the symptom checker live on the Desktop (`../Chartwise`, `../batchserve`, `../symptom-checker-app`); check claims against those repos (READMEs, benchmark/test output) rather than inventing or rounding numbers. The page also serves as a resume, so keep its facts consistent with the owner's current resume, which is the source of truth when they disagree.
