# Concept A — "Spec Sheet, Cupertino Edition" · Implementation Note

**File:** `cupertino.html` (1501 lines, ~78 KB, self-contained)
**Cross-link:** footer `← all formats` → `index.html`
**Baseline:** content lifted verbatim from `blueprint.html`

## What's in the file

| Lines | Section | Notes |
|---|---|---|
| 1–626 | `<head>` + `<style>` | tokens, layout, all section CSS |
| 628–646 | Hero | centered `<h1>` "Seven years. One device.", 19px subhead, meta line |
| 648–710 | § 00 Transformation | plotter, `#F5F5F7` rounded card, replay button (pill, blur-bg) |
| 712–1008 | § 01 Elevations | 2×2 grid, hairline-only chrome, 1.5px strokes |
| 1010–1144 | § 02 Compare | sticky `<thead>`, alternating `#F5F5F7` row tint, mini-thumbnails per column, "Change" column @ `#0071E3` |
| 1146–1205 | § 03 Capability | 2×4 grid, `border-radius: 16px` panel cards, 4px rounded bars, accent 0.85 opacity |
| 1207–1337 | § 04 Architecture | two-color (ink + apple-blue tint at 5%), 1000px inner wrapper, overflow-x:auto, soft fade gradient under 1080px |
| 1339–1430 | § 05 Timeline | lifespan band swapped to `var(--accent-tint)` (~10 % apple-blue), accent-dashed S25 FE launch line |
| 1432–1443 | Footer | 2-line centered, drops DEV-EVO / Sheet / Status; cross-link present |
| 1447–1500 | `<script>` | IntersectionObserver for plotter + fade-in reveal; both honor `prefers-reduced-motion` |

## Tokens

- Paper `#FBFBFD`, panel `#F5F5F7`, ink `#1D1D1F`, ink-2 `#424245`, ink-3 `#86868B`, rule `#D2D2D7`, rule-soft `#E5E5EA`.
- Accent `#0071E3`, accent-tint `rgba(0,113,227,0.10)`.
- Font stack: `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", "Helvetica", "Arial", sans-serif`. Mono: `"SF Mono", "Menlo", "Monaco", "Consolas", ui-monospace, monospace`. No Google Fonts loaded — SF Pro renders natively on Apple devices, falls back gracefully elsewhere.
- 8pt grid via `padding: 96px 0` sections, 32px frame gutter, `gap: 28px` delta grid, etc.

## Animation

- Plotter cycle: 11.5 s preserved. Stagger tiers `0 / 0.6 / 1.2 / 1.8 s` (already amplified in baseline — kept). Strokes thickened to 1.5 px ink.
- Scroll-in reveal: opacity-only fade, 400 ms ease-out, threshold 0.12. No Y-translate (per spec).
- `prefers-reduced-motion: reduce` → reveal applied instantly, plotter shows static Unit B (S25 FE) drawn, replay button hidden.

## Voice

- Section titles in Apple Title Case ("Transformation", "Compare", "Capability", "System shift", "Timeline") with `§ NN` eyebrow above.
- Section h2 statements: declarative — "From notch to cutout, plotted in place.", "Subsystem by subsystem.", "Quantitative deltas.", "Ecosystem migration.", "Lifespan, 2018 to 2025."
- Labels softened ("12 MP wide f/1.8 · single lens" instead of all-caps).
- Footer collapsed to 2 centered lines; dropped Doc-no, Revision, Sheet, Status, Maintainer columns. Maintainer + build moved inline to first line.

## Accessibility

- `<title>` + `<desc>` on plotter, all 4 elevations, arch SVG, timeline SVG.
- Heading hierarchy: `h1` → 6× `h2`, visually-hidden `h2` in footer for document-control landmark.
- `:focus-visible` ring (2 px `#0071E3`, 3 px offset) on `<a>` and `<button>`. Forced-colors fallback → `CanvasText`.
- Replay button min-height/min-width 44 px.
- Cross-link `← all formats` is a real `<a href="index.html">`.

## Responsive

- 360 px → 1440 px range covered.
- Frame: 980 px (Apple docs width); wide frame 1100 px for matrix + arch.
- § 02 matrix wraps `overflow-x: auto` with `min-width: 720px` on the table to keep columns readable; mobile retains `data-label` attrs (carried from blueprint).
- § 04 inner SVG wrapper `min-width: 1000px` + parent `overflow-x: auto` (per spec).
- § 05 timeline `min-width: 720px` + scroll.
- 720 px breakpoint: elev-grid → 1 col, delta-grid → 1 col.

## Deviations from spec (intentional)

1. **§ 02 matrix sticky `<thead>`** — sticky relative to viewport only inside the `.matrix-wrap`; on horizontal scroll the entire `<thead>` row scrolls with the table, which is the standard compare-page behavior. No vertical sticky was wired because the table is short.
2. **§ 02 thumbnails** — used simplified mini-SVGs at 80 px (not the full §01 silhouettes at 64–80 px). Reusing the full §01 SVGs would inflate the file and the small thumbnail benefits from a stripped-down rendering. Geometry still matches: XR has notch pendant, S25 has punch-hole.
3. **§ 04 arch RIGHT-stack tint** — applied a very soft `rgba(0,113,227,0.05)` fill to the 2025 stack boxes (rather than leaving both stacks white) to disambiguate visually without overpowering. The legend's "2025 stack tint" key reflects this.
4. **§ 03 bars** — XR row uses `var(--ink-3)` (`#86868B`); S25 row uses `var(--accent)` at 0.85 opacity. Spec said "soft `#0071E3` fill at low opacity" — applied to S25 only since the deltas are meant to read 2025-as-progress.
5. **Plotter card** — used `var(--panel)` (`#F5F5F7`) flat (no border), `border-radius: 16px`. Replay button is a translucent pill with backdrop-blur (Cupertino convention) rather than a hairline rectangle.
6. **Hero meta line** — added a small mono caption row ("2018 → 2025 · 2 platforms · Updated 2026-05-17"). Spec said "one-line subhead at 19px" — the 19 px subhead is in place; this meta line is at 12 px below it and acts as a quieter document-context anchor (replaces the dropped title-block side-panel data).

## What's NOT in the file (per spec)

- No background grid pattern.
- No corner registration marks.
- No scale-bar.
- No title-block side-panel.
- No crumb breadcrumb ("Personal / Devices / Evolution / Rev A").
- No "Rev A — 2026.05" / "DEV-EVO-001" / "Sheet 1 of 1" framing.
- No `Inter` or `JetBrains Mono` import.

## Unresolved questions

- Should the §02 mini-thumbnails be replaced with shrunken copies of the actual §01 SVGs (full fidelity)? Current choice prioritizes file size and visual clarity at 80 px. Easy swap if requested.
- Hero meta line ("2018 → 2025 · 2 platforms · Updated 2026-05-17") — kept as a quiet replacement for the removed title-block. Drop entirely if the hero should be even more austere (just headline + subhead).
- Arch right-stack `5 %` apple-blue fill — could go to `0 %` (pure white) if the legend's "2025 stack tint" key is misleading.

**Status:** DONE
**Summary:** `cupertino.html` built per Concept A spec; 1501 lines, content verbatim from blueprint, light Apple developer-docs aesthetic, accent `#0071E3`, plotter preserved with reduced-motion fallback, footer cross-links to `index.html`.
