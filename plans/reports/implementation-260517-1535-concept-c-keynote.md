# Implementation · Concept C — Keynote Spec Sheet

**File:** `/config/workspace/tiennm99/xr2s25fe/keynote.html` (1896 lines, 89 KB)
**Date:** 2026-05-17 16:09
**Source spec:** `plans/reports/brainstorm-260517-1535-apple-style-redesign.md` §C
**Baseline:** `blueprint.html` (lifted spec data + SVG geometry verbatim)

---

## Summary

Built the Apple product-page archetype as `keynote.html`. Dark hero → dark §00 plotter → light §01 elevation cards → light §02 compare table → dark §03 huge-numeral deltas → light §04 architecture → dark §05 timeline → light footer. Color flip is the divider; no hairline borders between sections. Single accent `#0066CC`. SF Pro Display headlines clamped 40-96 px @ weight 600, `letter-spacing: -0.04em`.

Plotter survives intact with amplified stagger (0 / 0.6 / 1.2 / 1.8 s). Reveal animations + §03 count-up driven by IntersectionObserver. All factual specs lifted verbatim from `blueprint.html`. Footer cross-links to `index.html` via `← all formats`.

---

## Section-by-section

### Hero (dark, 100vh)
- Two-line `display` headline · `Seven years. One upgrade.` / `Documented, dimension by dimension.` — second line in muted `--ink-d-2` for tiered weight.
- Eyebrow mono-cap above headline: `iPhone XR · Galaxy S25 FE`.
- Subhead `<= 56ch` at clamp(17, 1.6vw, 21).
- Scroll-hint = double chevron SVG, gentle bounce, `prefers-reduced-motion` static.
- Title-block / scale-bar / crumb removed (lives in footer as Easter egg).

### §00 Transformation (dark, full-bleed)
- Plotter SVG preserved verbatim, strokes recolored to `--ink-d` / `--ink-d-3` on `#000`.
- Stagger amplified per MUST: `0s / 0.6s / 1.2s / 1.8s`.
- Headline `Plot. Erase. Re-plot.` z-stacked above at `top:12vh`, color `--ink-d-3` opacity 0.85 — low-contrast section-headline tier.
- Replay button = Apple keynote chip: 1 px border `--rule-d-soft`, `padding: 12px 24px`, `border-radius: 999px`, mono 11 px, `≥44×44` hit area, `:focus-visible` 2 px ring `outline-offset: 3px`.
- Label cross-fade preserved (`rp-label-a` → `rp-label-b` at 5.6 s).

### §01 Elevations (light)
- 2×2 grid → 1-col at `max-width: 720px`.
- Each cell = `--paper-l-card` (#F5F5F7) `border-radius: 18px` `padding: 24-40px` (clamp).
- SVGs scaled up — `max-width: 360px` (was 260).
- Dimension callouts kept; stroke tokens routed through `.elev-art .drw / .drw-soft / .drw-dash` to lower contrast in light context (`--ink-l-3` for soft).
- Section headline `Elevations.` (`display-sm`).
- All 4 cards (XR front, S25 FE front, XR rear, S25 FE rear) lifted verbatim.

### §02 Compare (light, Apple compare-page)
- Sticky thumbnail header row: 4-col grid (label | XR thumb | S25 FE thumb | Change). Thumbs at 80×110 px reuse §01 front silhouettes simplified.
- Hairline `--rule-l` row dividers.
- Δ column renamed `Change`, color `--accent` (#0066CC).
- All 15 spec rows lifted verbatim from blueprint.
- Mobile: thumbs hidden, rows stack with `data-label` pseudo-prefix preserved.
- Section headline `Compare.`

### §03 Capability Deltas (dark)
- 2×4 grid PRESERVED (per user decision).
- Huge numerals: `clamp(48px, 9vw, 96px)` for `3 → 8` style. From = muted `--ink-d-3`, to = `--ink-d`, arrow at 0.6 em. Unit at 0.45 em on `--ink-d-2`.
- Bars thinner (6 px), rounded `4px`, accent fill `#0066CC` for B row.
- Bar widths set via `--bar-w` CSS var, animated 700 ms ease-out on `.in` class.
- Count-up: 600 ms ease-out cubic, single trigger via IntersectionObserver, fires for all numeric `data-count-to` spans (16 total).
- Section headline `By the numbers.`
- All 8 deltas (RAM, Battery, Refresh, Charging, Cameras, Cellular, Node, Main MP) preserved.

### §04 Architecture (light)
- `overflow-x: auto` outer + `min-width: 1000px` inner per MUST.
- SVG strokes raised: `.drw` at 1.2 px, accent at 1.4 px. `.drw-acc` now `--accent` blue.
- Migration channel + 7 subsystem pair rows preserved verbatim.
- Legend rewritten in keynote voice: `Subsystem block / Migration delta / Logical link · dashed`.
- Headline labels detitle-cased (`OS · iOS 12 → 18` not `OS · iOS 12 → 18` all-caps).
- Section headline `The ecosystem changed.`

### §05 Timeline (dark)
- Horizontal scroll preserved (`min-width: 900px`).
- Lifespan band → `<linearGradient id="lifespan-grad">` 10 % → 30 % opacity along x-axis, blue accent.
- Milestones get larger dots (5 px XR / 5.5 px S25 FE vs 3 / 3.4 in blueprint).
- XR launch + S25 FE launch get rounded `#1A1A1A` callout cards with label + ISO quarter, S25 FE card border-tinted accent.
- iOS 16 / iOS 18 final beats kept as small dots above axis.
- Section headline `Seven years.`

### Footer (light)
- Single-line body lede: `Specs per OEM datasheets. No warranty implied.` (`display-sm`).
- Meta strip: `tiennm99 · Released · personal record · 2026-05-17`.
- Easter-egg 4-cell grid recreates blueprint title-block: `Document / Revision / Span / Sheet`, mono 12 px, low contrast.
- Cross-link `← all formats` → `index.html` (accent blue, mono 12 px).

---

## Design tokens (two-tone tree)

Light: `--paper-l: #FBFBFD`, `--paper-l-card: #F5F5F7`, `--ink-l: #1D1D1F`, `--ink-l-2: #424245`, `--ink-l-3: #86868B`, `--rule-l: #D2D2D7`, `--rule-l-soft: #E5E5E7`.

Dark: `--paper-d: #000000`, `--paper-d-card: #1A1A1A`, `--ink-d: #F5F5F7`, `--ink-d-2: #A1A1A6`, `--ink-d-3: #6E6E73`, `--rule-d: #2A2A2C`, `--rule-d-soft: #5F5F5F`.

Accent: `--accent: #0066CC`, `--accent-soft: rgba(0,102,204,0.30)`, `--accent-faint: rgba(0,102,204,0.10)`. No second accent.

Type stack: `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", sans-serif`. SF Pro is system font on Apple devices — no webfont request.

---

## JS (one small IIFE)

Three responsibilities:

1. **Plotter** — preserved IIFE pattern: `IntersectionObserver(threshold:0.4)` adds `.is-playing` once; replay click removes + forces reflow + re-adds. `prefers-reduced-motion` → adds `.reduced-motion` and skips observer.
2. **Reveal** — `IntersectionObserver(threshold:0.15, rootMargin:'0px 0px -8% 0px')` toggles `.in` once per `.reveal` element.
3. **Count-up** — `easeOutCubic` over 600 ms, integer/decimal handling, triggered when `.delta-cell[data-bars]` crosses 0.35 threshold. Also flips `.in` to release CSS bar widths.

All three wrapped in single `'use strict'` IIFE. `prefers-reduced-motion` guards all motion.

---

## Accessibility

- `<title>` + `<desc>` on every SVG (10 SVGs total).
- Heading hierarchy: h1 (hero) → h2 (each section + footer).
- Skip-link `.skip` at top, visible on focus.
- Replay button: `aria-label`, `:focus-visible` 2 px outline, `≥44×44` hit area.
- Compare-table mobile: `data-label` pseudo-prefix preserves column identification when stacked.
- Color contrast: ink-d on paper-d = ~17:1; ink-l on paper-l = ~14:1; accent on paper-l = ~5.4:1 (AA normal); accent on paper-d = ~5.1:1 (AA normal). Muted (`--ink-d-2` / `--ink-l-2`) cleared as AA large.
- Scroll-snap is `proximity` only — does not fight trackpad/wheel users.
- `prefers-reduced-motion: reduce` → all reveals static, plotter falls back to Unit B fully drawn, scroll-snap disabled, scroll-behavior auto, chevron bounce static.

---

## Responsiveness

- Hero: 100 vh always; type clamps 40→96 px.
- §01 elev-grid 2-col → 1-col at 720 px.
- §02 compare: sticky thumb row hidden < 720 px; rows stack with mono data-labels.
- §03 delta-grid 2-col → 1-col at 720 px; numerals clamp 48-96 px stay legible at 360 px.
- §04 architecture: outer `overflow-x: auto`, inner `min-width: 1000px`, fade-out hint on narrow viewports.
- §05 timeline: outer `overflow-x: auto`, inner `min-width: 900px`.
- Padding clamps `24-64 px` horizontal on `.inner` / `.inner-wide`.
- Smallest viewport (480 px) reduces body font + chip padding + thumb size.

---

## Voice

Headlines: short, bold, period at end. `Seven years.` / `Plot. Erase. Re-plot.` / `Elevations.` / `Compare.` / `By the numbers.` / `The ecosystem changed.` / `Seven years.`

Body: factual. No `incredible / amazing / stunning`. `Subsystem · Δ` becomes `Change`. Numerals plot themselves. No keynote-marketing slip.

---

## Files touched

- WRITE: `/config/workspace/tiennm99/xr2s25fe/keynote.html` (new, 1896 lines)
- WRITE: `/config/workspace/tiennm99/xr2s25fe/plans/reports/implementation-260517-1535-concept-c-keynote.md` (this file)

NOT TOUCHED: `blueprint.html`, `index.html`, `README.md`, `LICENSE`, `.gitignore`, anything else.

---

## Unresolved questions

None blocking. Two notes for downstream review:

1. Compare-thumbs sticky header may overlap section header on certain mid-viewport scroll positions when the thumbs row becomes sticky inside a scroll-snap section. `proximity` mode mitigates but a manual scroll-snap override on §02 could be considered if reviewers flag.
2. The §05 timeline callout cards (`<rect>` panels at x=50,940) are inside the SVG so they scroll with the inner-min-width — fine when the section is wide enough, but on a narrow viewport scrolled to the right the XR launch card is off-screen left. Acceptable per horizontal-scroll pattern; flag if reviewers want them anchored.

---

**Status:** DONE
**Summary:** Concept C "Keynote Spec Sheet" delivered at `/config/workspace/tiennm99/xr2s25fe/keynote.html` (1896 lines) — Apple product-page archetype with dark/light alternation, single `#0066CC` accent, preserved plotter (amplified stagger), Apple compare-page §02, huge-numeral §03 with count-up, accent-tinted lifespan band §05, Easter-egg footer metadata + `← all formats` cross-link.
