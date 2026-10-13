# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What it is

Ryvath Mattey's personal portfolio page, `index.html`.

It is a single self-contained static page: all CSS is in one `<style>` block in `<head>`, and the only JS is a small `IntersectionObserver` at the bottom of `<body>`. There is no build step, package manager, linter, or test suite. External dependencies are limited to Google Fonts (DM Serif Display, DM Mono, Outfit).

`resume.pdf` is a copy of `~/Documents/RyvathMatteyResumes/RESUME_RMattey.pdf`; recopy it when the resume changes.

Preview: `open index.html`, or `python3 -m http.server` from this folder and visit `http://localhost:8000`.

## Structure and conventions

- **Design tokens** live on `:root` (`--bg`, `--surface`, `--accent` purple, `--accent2` teal, `--serif`/`--sans`/`--mono`). The page is dark-only; use these variables, not new hex values.
- **Sections** follow one pattern: `<section id="x">` → `.section-label` ("NN / x", numbered in order) → `.section-title` → content, separated by `<div class="divider">`. The nav links (`.nav-links`) point to section ids, so adding, removing, or renaming a section means updating the nav and renumbering the labels.
- **Projects** are `.project-card`s in `.projects-grid` (2 columns). Each has a two-digit `.project-number` (renumber on insert or reorder), `.stack-pill`s, `.project-bullets` (with `<strong>` around key terms and metrics), and `.project-links`. `.featured` makes a card span the full width and puts its bullets in two columns. Each card also has a `data-tech` attribute (`|`-separated): the skills it uses, spelled exactly as the `.tag`s in the Skills section. A script turns any skill tag that appears in some card's `data-tech` into a button that filters the projects, so update `data-tech` when a project or skill changes.
- **Experience** entries are `.exp-item` grids (meta column + role/bullets).
- **Animation:** any element with `.fade-in` starts hidden and is revealed by the observer script. New top-level blocks should carry `.fade-in` to match; an element with `.fade-in` that the script never observes stays invisible.
- **Responsive:** the main `@media (max-width: 768px)` block collapses the grids to one column and swaps the nav links for a `.nav-toggle` menu. Add mobile overrides there for any new multi-column layout. A separate `max-width: 1000px` block stacks the Chartwise pipeline diagram vertically, since its 7-stage row gets cramped earlier.

## Content accuracy

Project bullets quote specific metrics (test counts, throughput, latencies, accuracy). Chartwise, batchserve, and the symptom checker live on the Desktop (`../Chartwise`, `../batchserve`, `../symptom-checker-app`); check claims against those repos (READMEs, benchmark/test output) rather than inventing or rounding numbers. The page also serves as a resume, so keep its facts consistent with the owner's resume.
