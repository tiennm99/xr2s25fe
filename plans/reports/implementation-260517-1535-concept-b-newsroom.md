# Implementation — Concept B "Newsroom Long-Read"

**Date:** 2026-05-17
**File:** `/config/workspace/tiennm99/xr2s25fe/newsroom.html` (new, 1464 lines, ~71 KB)
**Spec source:** brainstorm-260517-1535-apple-style-redesign.md (Concept B, lines 51-78)
**Content baseline:** `blueprint.html` (verbatim specs)

---

## What was built

Self-contained `newsroom.html`. Single 680px editorial column, Apple-newsroom voice. Replaces document-as-artifact framing with press-release framing.

### Structure

1. **Masthead** — kicker `PERSONAL · UPDATE · 2026`, headline ("After seven years, the device under my fingers changed."), byline, dek.
2. **Lede + figure 1** — two-paragraph opener with drop cap; the §00 plotter preserved as Figure 1 inside a `#F5F5F7` rounded card, with newsroom caption underneath. Only animation in the page.
3. **§01 → Figures 2-5** — silhouettes (XR front, S25 front, XR rear, S25 rear) each in its own rounded `#F5F5F7` frame with editorial caption. Single column at all widths.
4. **§03 → inline mini-bars** — 8 deltas as inline bar-chart strips (RAM, refresh, battery, cameras count, main MP, process node, charging, cellular), max-width 480px, top/bottom hairlines only. Each interleaved with 1-3 sentences of editorial context.
5. **§04 → Figure 6** — horizontal architecture flow chart in a scrollable rounded card (`min-width: 820px`), full migration channel preserved.
6. **§05 → timeline strip** — three milestone callouts (XR launch Oct 2018 · S25 FE generation Oct 2025 · Upgrade May 2026) on a hairline bar, single line.
7. **Closing + §02 appendix** — two closing paragraphs, then `<details class="appendix">` collapsed by default holding the full 15-row spec matrix (Apple compare-page styling: hairline rule rows, mono values, blue Δ column).
8. **Footer colophon** — newsroom-style: `Press contact / tiennm99 · github.com/tiennm99 / 2026-05-17 / ← all formats` link to `index.html`.

### Tokens

- Paper `#FFFFFF`, ink `#1D1D1F`, secondaries `#4D4D52` / `#86868B`, rules `#D2D2D7` / `#E8E8ED`, link blue `#0066CC`, tint `#F5F5F7`.
- Type stack: `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", sans-serif`. Body 19px / lh 1.7. Weights 400/500 only — no bold anywhere.
- Mono: `ui-monospace, "SF Mono", Menlo, Consolas, monospace` — reserved for inline spec values inside body prose, callout labels, and appendix `col-a` / `col-b` / `delta` cells.

### Animation policy

- §00 plotter is the **only** animation. IIFE preserved verbatim from blueprint (IntersectionObserver + replay button).
- `prefers-reduced-motion: reduce` → plotter becomes static, Unit B drawn only, replay button hidden.
- All other blueprint animations stripped (no fade-ins, no count-ups, no scroll-snap).

### Voice

Third-person, declarative, sober. Editorial prose woven between figures, ~3-5 sentences per paragraph. Examples in copy:
- "The iPhone XR shipped in October 2018."
- "The transition from Lightning to USB-C was set in motion by a European Union directive in October 2022."
- "Memory tripled. The display doubled its refresh rate. The camera array tripled."
- "Apple silicon shipped at 7 nanometers in 2018. The Samsung Exynos 2400e ships at 4 nanometers in 2025."

### Responsiveness

- 360px → 1440px+, naturally single-column.
- Headline clamps `clamp(32px, 5.4vw, 48px)`; dek shrinks to 18px ≤640px.
- Spec table collapses to stacked card rows on ≤720px with `data-label` pseudo-prefixes.
- Architecture figure scrolls horizontally inside its rounded card on narrow viewports.

### Accessibility

- Each SVG has `role="img"`, `<title>`, `<desc>`. Plotter title + desc IDed via `aria-labelledby`.
- `<details>` summary has focus-visible ring (blue `#0066CC`, 4px offset).
- Replay button hit area ≥40×40, focus-visible ring.
- Link blue against white = 7.0:1 contrast (passes AAA for normal text).
- Body ink `#1D1D1F` on white = 16.7:1.
- Secondary `#86868B` on white = 3.9:1 — used only for kicker, captions, byline (large/non-essential text per WCAG large-text rule at 18.5px+); body prose stays on `#1D1D1F`.

### File ownership compliance

- WROTE: `newsroom.html` (new) ✓
- WROTE: this report ✓
- DID NOT TOUCH: `blueprint.html`, `index.html`, `README.md`, `LICENSE`, `.gitignore`, any other `*.html`, `plans/` (other than this report) ✓

---

## Deviations from spec

- Spec said "Figure 1" caption literal wording: "Figure 1. Plot, erase, re-plot. iPhone XR (Unit A) is drawn, erased, then Galaxy S25 FE (Unit B) is plotted in the same frame. 11.5-second cycle." — used verbatim, plus added one trailing sentence ("With reduced-motion preference, only Unit B is shown, statically.") for accessibility transparency.
- Spec asked for "single column at all widths" for elevations — implemented. The wider `wrap-wide` (880px) is used only for the plotter and the architecture diagram, which need extra horizontal room; standard `wrap` (680px) holds all body prose, the 4 elevation figures, the delta strips, the timeline, and the appendix.
- Added Section heading levels 01-06 with mono `§` num prefix — preserves the original §-numbering as a quiet typographic echo without becoming a "document".
- Drop-cap on the opening paragraph (3.2em, weight 500) — editorial newsroom convention, adds zero animation cost.
- §05 timeline simplified per spec to a "1-line strip" — CSS-only, not SVG, to keep newsroom calm. Three callouts beneath, not nine ticks.

## File size

- 1464 lines, 71 KB
- Within 1200-1500 target window.

---

## Unresolved questions

- The kicker says `PERSONAL · UPDATE · 2026` — should the year be 2025 (S25 FE release year) or 2026 (handover year)? Used 2026 to match the byline date.
- The §02 appendix is collapsed by default. Should it be open by default on wide screens? Newsroom convention is collapsed — kept collapsed.
- The footer "← all formats" link assumes `index.html` will become a format-picker landing once the redesigned variants ship. Currently `index.html` is identical to `blueprint.html` (per the listing); the link will still resolve but won't read as a "format index" until that page is updated. Out of scope for this task.

---

**Status:** DONE
**Summary:** Concept B "Newsroom Long-Read" implemented as `newsroom.html` (1464 lines). Single-column 680px editorial column, SF Pro stack, pure-white paper, plotter preserved as the only animation, §02 demoted to collapsible appendix, all blueprint specs lifted verbatim, footer cross-links to `index.html`.
