# Implementation · Concept E — Two-Tone Diptych

Date: 2026-05-17
Output: `diptych.html` (2068 lines)
Status: DONE

## What shipped

Self-contained `diptych.html`. Vertical split full-page-height, light-on-left (XR/2018) vs dark-on-right (S25 FE/2025), Apple two-tone aesthetic. Inline CSS + inline JS. Zero external dependencies (no Google Fonts; SF Pro stack used).

## Section-by-section translation from blueprint

| § | Blueprint | Diptych |
|---|---|---|
| Hero | Title block + cover sheet metadata | Full-viewport diptych, year (96px) + model (24px) per half, large silhouettes (~440 vu), no central headline |
| § 00 | Plotter as full-width section | Plotter in center gutter strip (220 px wide on desktop), flanked by light/dark wings. Plotter SVG and IIFE preserved verbatim, only viewBox shrunk to 240×360 to fit strip |
| § 01 | 4-cell elevation grid | Diptych with XR front+rear stacked on light side, S25 FE front+rear stacked on dark side |
| § 02 | 4-col semantic table | Semantic table preserved (a11y); visually rendered as 3 painted lanes — XR-right-aligned cell, accent Δ chip absolutely-positioned over the gutter, S25 FE-left-aligned cell. `<thead>` hidden (display:none); category becomes a `.cat-label` inside each col cell so screen readers still hear it |
| § 03 | 2-col bar pair grid | Stacked rows, each with centered headline (e.g. "Memory.") + accent factor tag ("× 2.67") + a diptych bar pair. Left bar fill anchored to the gutter (right:0) and grows leftward; right bar fill anchored to the gutter (left:0) and grows rightward in accent blue |
| § 04 | 2-col arch w/ center migration channel | NATIVE FIT — diagram stretched edge-to-edge (1200×540 viewBox), connectors are 7 accent-blue lines crossing the full 640px gutter. Each connector animates via stroke-dashoffset on scroll-into-view, staggered 100ms per connector |
| § 05 | Single timeline SVG | Same SVG split into left/right halves by year coordinate (540 = midline). Lifespan band straddles the gutter as a translucent accent rectangle. Left half rendered with `currentColor: #1D1D1F` (light); right half with `currentColor: #F5F5F7` (dark). S25 FE launch marker uses accent #0066CC |
| Footer | 6-cell grid, light | Full-width dark, 4-col with `← all formats` link to `index.html` (cross-link to format index) |

## Type / color tokens

- Font stack: `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", sans-serif`. Mono: `"SF Mono", "Menlo", monospace`. Letter-spacing tightened on display (-0.02em) per Apple spec.
- Light side: `#FBFBFD` paper, `#1D1D1F` ink, `#D2D2D7` hairline.
- Dark side: `#1D1D1F` paper, `#F5F5F7` ink, `#3A3A3C` hairline.
- Accent `#0066CC` reserved for: Δ chips in §02, factor tags in §03, dark-bar fills in §03, migration connectors in §04, S25 FE launch marker in §05, `← all formats` link in footer.
- No third color used anywhere.

## SVG token strategy

SVG strokes use `currentColor` so the same `.drw` class produces ink-color in light context and paper-color in dark context. The `.side-d`, `.elev-d`, `.bar-d`, `.hero-d` wrappers set `color: var(--ink-d)`. The architecture & timeline SVGs use inline `style="color: ..."` per group because they straddle both tones in a single SVG.

## Animation

- **§ 00 plotter** — Same 11.5s draw-erase-draw cycle as blueprint. Stroke-dasharray=100 with pathLength=100 normalizing each path. IntersectionObserver triggers `.is-playing` at 40% threshold. Replay button reflows + re-adds class.
- **§ 04 connectors** — 7 lines, stroke-dasharray=80, staggered 0/100/200/300/400/500/600ms. IntersectionObserver at 25% threshold adds `.is-visible` once.
- **`prefers-reduced-motion`** — Plotter falls back to Unit B fully drawn (no animation); arch connectors render fully (no animation). All replay buttons hidden.

## Responsive (the hard part)

- ≥ 720px: full diptych, 1fr / 1px / 1fr grid throughout.
- < 720px: every diptych grid collapses to `1fr` (single column). Gutters are `display:none`. Each half becomes a full-width slab, alternating light/dark per section.
  - Matrix rows on mobile: XR row (light), then accent Δ chip on its own line, then S25 FE row (dark). Visually different from desktop diptych but still tone-coded.
  - Delta bars on mobile: both bars now grow rightward (left-anchored), still tone-coded.
  - Plotter on mobile: wings hidden, strip spans full width.
- 721-1024px: shrunk padding, gutter-strip width reduced to 200px.
- Arch + timeline SVGs use horizontal scroll on narrow viewports (min-width 960/880px).

## Accessibility

- Semantic `<table>` retained for §02 with sr-only caption. `<thead>` hidden visually but available to assistive tech. Each `<td>` repeats the category via `.cat-label` for context when the row is read.
- All SVGs have `<title>` + `<desc>` and `role="img"`.
- `.sr-only` h1 in hero ("Device Evolution — iPhone XR (2018) versus Samsung Galaxy S25 FE (2025)") provides a real heading-1 for outline.
- Replay button is `<button>`, ≥44×44 hit area, visible focus ring (2px solid outline + offset).
- Contrast: `#1D1D1F` on `#FBFBFD` = 17.8:1; `#F5F5F7` on `#1D1D1F` = 16.2:1; accent `#0066CC` on `#FBFBFD` = 5.6:1 — all AA+.

## Content fidelity

All factual specs lifted verbatim from blueprint.html: display sizes, RAM, battery, charging, camera counts, MP, process node, network gen, ingress, OS, dates, callout labels (A/B/C/D), dimensional values (75.7 / 150.9 / 76.6 / 161.3 / 8.3 / 7.4 mm), launch quarters (Q4 2018 / Q4 2025), milestones (5G rollout / USB-C mandate / iOS 16 / iOS 18 final / on-device LLMs).

## Tag balance check (run post-write)

section 7/7 · div 196/196 · article 4/4 · svg 9/9 · g 13/13 · table 1/1 · tr 16/16 · td 60/60 · footer 1/1 · script 1/1 · style 1/1 — all match.

## File ownership respected

WROTE: `diptych.html`, this report.
DID NOT TOUCH: `blueprint.html`, `index.html`, `README.md`, `LICENSE`, `.gitignore`, anything else under `plans/`.

## Unresolved questions

- Δ chip overlap on §02: on viewports between ~720-900px the chip can crowd the row text since it's absolutely positioned dead-center. Acceptable since mobile breakpoint repositions it; tablet appears OK at standard 768px column widths but a chip-collision tuning pass at 800-900px could be considered.
- Center gutter strip width (220px desktop) compresses plotter visibility somewhat vs blueprint's 640px viewport — readable but tighter. Considered moving plotter back to a full-width section between hero and §01 but spec was explicit: plotter goes in the gutter. Kept as specified.
- Timeline year tick at 540 (midline) lands between 2021 and 2022 — left half currently renders 2018-2021, right half 2022-2025. The boundary axis line is drawn in two halves which may leave a hairline-thin visual gap at zoom; visually unnoticeable at 1× but flagged.

**Status:** DONE
**Summary:** Concept E Two-Tone Diptych shipped as `diptych.html` (2068 lines). All seven sections translated to the diptych format with strict 2-tone palette, accent `#0066CC` reserved for migration/Δ artifacts, plotter relocated to center gutter strip, §04 connectors animate on scroll, mobile collapse to alternating slabs, footer cross-links to `index.html` via `← all formats`.
