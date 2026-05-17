# Implementation note — §01 elevations SVG fidelity fixes

File modified: `/config/workspace/tiennm99/xr2s25fe/index.html`
Audit ref: `plans/reports/review-260517-1450-svg-fidelity.md`

## Summary of changes

Reworked §01 entirely. Was: 2-cell row, front elevations only, both bodies drawn at ~3.5:1 (remote-control proportions), rogue rear-cam on XR front, undersized v-notch, no rear views, S25 buttons on both sides, no speaker/mic grilles. Now: 2×2 grid (XR front · S25 front · XR rear · S25 rear), all bodies at correct ratios, distinct industrial-design deltas visible.

## Line ranges touched

| Region | Old lines | New lines | Change |
|---|---|---|---|
| `.elev-grid` / `.elev-cell` CSS | 172–205 | 172–211 | rules for `:nth-child(2n)` right-border + `:nth-last-child(-n+2)` bottom-border; svg max-width 280 → 260 |
| §01 section markup | 457–592 | 460–774 | full rewrite — 4 elevation cells, new group classes, `pathLength="100"` everywhere, dimension/callout/feature separation |

Everything outside §01 (and the small CSS block above) is untouched. §02 matrix, §03 deltas, §04 architecture, §05 timeline, footer — all preserved verbatim.

## Audit fix-list status

| # | Audit fix | Status |
|---|---|---|
| 1 | Body aspect ratios — XR 2.0:1, S25 2.10:1 | DONE — XR 100×200, S25 104×219, viewBox shrunk 260×480 → 260×360 to remove dead space |
| 2 | Remove rogue rear-cam circle from XR front | DONE — gone. Callout `B · 12 MP f/1.8` retired from front and migrated to XR rear |
| 3 | Redraw XR notch as trapezoidal pendant ⅓ of screen width | DONE — notch traces from (116,96)→(120,104)→(140,104)→(144,96), ~28 px wide on 84 px screen (33%), 8 px deep, rounded shoulders via Q-curves. Inside-notch detail (cam dot, IR dot, speaker slit rect) repositioned to cy=100 — inside trapezoid body |
| 4 | Add S25 FE rear elevation, 3 discrete rings, no island | DONE — three vertical rings at cy=110/130/150, x=100. Outer radii 6 / 7 / 6.5 (telephoto / wide-main / ultrawide). Each ring = `<circle class="drw …-outer">` + `<circle class="drw-soft …-inner">`. NO surrounding island rect. Flash + mic dots beside middle ring. SAMSUNG wordmark bottom |
| 5 | Add iPhone XR rear elevation, single-lens island | DONE — 22×22 rounded square island top-left, one lens (outer + inner), one flash, mic dot outside island. Apple-mark placeholder circle centered mid-body, "iPhone" wordmark lower-third |
| 6 | S25 FE side buttons — right only | DONE — removed `x1="78"` left-side line. Right side: volume rocker drawn as a 2×22 rect (longer/thicker than power) + 2×14 power rect below with gap. Both `drw` (not drw-soft) |
| 7 | XR mute switch + correctly differentiated volume buttons | DONE — left side now: 3×7 mute slider rect (top), short vol-up line, gap, short vol-down line. Right side: single power-button line shortened to 20 px (y=138→158). All `drw` weight |
| 8 | Speaker + mic grilles flanking ports | DONE — 3-dot grille left + 3-dot grille right of both Lightning and USB-C, `drw-soft`. New callout `D · LIGHTNING · SPK / MIC` and `D · USB-C · SPK / MIC` |
| 9 | Lighten / scale notch interior detail | DONE — implicit; cam/IR dot r=1, speaker slit rect 6×1.6 rx=0.8, all `drw-soft` |
| 10 (optional) | Side profile mini-elevation | SKIPPED — explicitly optional in audit, and 2×2 grid already fills the section. Can be added in a third row by a follow-up if desired |

## Differentiated industrial-design deltas now visible

- XR rx=14 (rounded aluminum) vs S25 rx=10 (flatter rails) on body corners
- XR notch (trapezoidal pendant) vs S25 punch-hole (centered circle)
- XR single rear lens inside island vs S25 three discrete rings no island
- XR mute switch on left edge vs S25 right-edge-only buttons
- Bezel thickness — XR 4 px (slightly thicker) vs S25 4 px outer + 4 px inner = nearly bezel-less impression
- Port type label retained on both (Lightning vs USB-C)

## Group / class naming reference (for animation agent)

Top-level groups — wrap each elevation. Animate one at a time:
- `.unit-a-front` — iPhone XR front
- `.unit-a-rear`  — iPhone XR rear
- `.unit-b-front` — Galaxy S25 FE front
- `.unit-b-rear`  — Galaxy S25 FE rear

Feature-level classes (consistent across all four views where applicable):

**XR front**
- `xr-body`, `xr-bezel`, `xr-screen`, `xr-notch`
- `xr-notch-cam`, `xr-notch-ir`, `xr-notch-speaker`
- `xr-btn-power`, `xr-btn-mute`, `xr-btn-volup`, `xr-btn-voldn`
- `xr-grille-left`, `xr-port`, `xr-grille-right`
- `xr-callout-notch`, `xr-callout-mute`, `xr-callout-screen`, `xr-callout-port`

**XR rear**
- `xr-rear-body`, `xr-rear-inset`
- `xr-cam-island`, `xr-cam-lens-outer`, `xr-cam-lens-inner`, `xr-cam-flash`, `xr-cam-mic`
- `xr-mark` (Apple-mark placeholder)
- `xr-rear-btn-power`, `xr-rear-btn-mute`, `xr-rear-btn-volup`, `xr-rear-btn-voldn`
- `xr-rear-callout-lens`, `xr-rear-callout-flash`, `xr-rear-callout-island`

**S25 front**
- `s25-body`, `s25-bezel`, `s25-screen`
- `s25-punchhole`, `s25-fp`
- `s25-btn-vol`, `s25-btn-power`
- `s25-grille-left`, `s25-port`, `s25-grille-right`
- `s25-callout-punch`, `s25-callout-fp`, `s25-callout-screen`, `s25-callout-port`

**S25 rear**
- `s25-rear-body`, `s25-rear-inset`
- `s25-cam-top-outer`, `s25-cam-top-inner` (telephoto)
- `s25-cam-mid-outer`, `s25-cam-mid-inner` (wide/main, largest)
- `s25-cam-bot-outer`, `s25-cam-bot-inner` (ultrawide)
- `s25-flash`, `s25-mic`
- `s25-rear-btn-vol`, `s25-rear-btn-power`
- `s25-callout-top`, `s25-callout-mid`, `s25-callout-bot`, `s25-callout-rings`

Shared / generic across all four:
- `dim-top` — top dimension callout group (3 dashed lines + text)
- `dim-side` — side dimension callout group
- `dim-island` (XR rear) — camera-bump dimension
- `dim-rings` (S25 rear) — ring-spacing dimension

## Animation primer

Every stroked `<line>`, `<rect>`, `<circle>`, `<path>` in §01 carries `pathLength="100"`. To draw with the schematic re-plot effect, the follow-up agent only needs:

```css
.elev-art [pathLength="100"] {
  stroke-dasharray: 100;
  stroke-dashoffset: 100;
  animation: drw 0.8s ease-out forwards;
}
@keyframes drw { to { stroke-dashoffset: 0; } }
```

… with per-group `animation-delay` cascades. `<text>` elements deliberately have no pathLength — animate via opacity fade-in after the surrounding shapes complete. Dimension groups (`.dim-top`, `.dim-side`) can fire first to read as construction lines; body outline → bezel → screen → features → callouts is the natural cascade order. All shapes use `vector-effect="non-scaling-stroke"` so the dasharray math stays consistent regardless of responsive viewport scale.

## Validation done

- All 6 inline SVG blocks parse cleanly via `xml.etree.ElementTree` (confirmed)
- No external assets added — file remains self-contained
- Aesthetic constraints preserved: only `#FAFAF7`, `#1A1A1A`, `#3D5A6C` accent (accent unused in elevations per audit); `drw` / `drw-soft` / `drw-dash` classes only; Inter + JetBrains Mono via existing Google-fonts link
- Responsive: `.elev-grid` collapses to single column < 720 px; svg `max-width: 260px` with `width: 100%` so cells scale gracefully on small screens
- No new colors, no neon, no gradients, no glassmorphism — pure blueprint ink

## Items deliberately skipped or postponed

1. **Side profile mini-elevation (audit fix #10)** — explicitly optional. 2×2 grid is visually balanced; adding a 3rd row would force a 4×1 or asymmetric layout. Recommend follow-up only if user wants the thickness-delta visualization (XR 8.3 mm rounded vs S25 7.4 mm flat rails).
2. **Earpiece slit at top of S25 screen** — audit noted this is "very subtle, not critical" on real S25 FE. Skipped to avoid clutter; current top edge of AMOLED area is clean.
3. **SIM tray indicator on S25 left edge** — audit said "optional detail". Skipped; S25 FE still has physical SIM but it's not a defining visual.
4. **Earpiece slit inside XR notch** — implemented as small `xr-notch-speaker` rect at (127, 99.2), 6×1.6 px. Audit said real earpiece is "~10×1.5 px" — used 6×1.6 to fit comfortably within the 28-px-wide notch trapezoid without crowding the cam/IR dots.

## Unresolved questions

1. Should we add the optional **side-profile row** (audit fix #10)? Would add thickness-delta + flat-vs-curved-rail visual. Requires a third row in `.elev-grid` (likely 4-column flat row of thin horizontal silhouettes, or 2-cell row matched to fronts/rears). Not blocking.
2. The XR rear "Apple-mark" placeholder is a `drw-soft` circle — should it be replaced with an actual outline-Apple SVG path? Currently a circle reads as a generic logo placeholder; an apple silhouette would be unambiguous but adds graphic noise that other blueprint elements avoid.
3. The S25 rear `dim-rings` label "≈ 40" represents the y-distance between top ring (cy=110) and bottom ring (cy=150) in SVG units, not millimeters. Should this be relabeled in mm-equivalents (e.g., ≈ 30 mm) to match the other dim labels? Currently consistent with the abstract "≈ 22" island-dim on XR rear, which is also SVG-units, but blueprint convention would expect mm.

---

**Status:** DONE
**Summary:** All 9 prioritized audit fixes applied; §01 now shows 4 elevations (XR + S25 FE, front + rear) at correct 2.0:1 and 2.10:1 aspect ratios, with single-lens XR island vs 3-ring S25 rear deltas, proper trapezoidal XR notch, right-only S25 buttons, XR mute slider, port grilles. Geometry is split into per-feature elements with `pathLength="100"` and semantic group classes ready for stroke-dasharray re-plot animation.
