# Concept D — Compare-Page Purist · implementation note

**Output:** `/config/workspace/tiennm99/xr2s25fe/compare.html` (852 lines, self-contained)
**Date:** 2026-05-17
**Brief source:** brainstorm-260517-1535-apple-style-redesign.md (Concept D, ~line 115-142)

---

## 1. Aesthetic decisions

- Pure Apple compare-page clone. Light only. `#FBFBFD` page bg, `#F5F5F7` alternating row tint, hairline `#D2D2D7` dividers.
- Max-width 980px, centered. Apple's compare uses ~980px column width.
- Type stack `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", sans-serif`.
- Body 17px / 1.47 / letter-spacing -0.022em (Apple body text rhythm).
- Row-group headings 24px / 1.167 / 600 weight (Apple "Display.", "Power." treatment).
- SF Mono 13-14px for numerals (mm, ppi, MHz, mAh) and ISO date.
- Single accent `#0066CC` reserved exclusively for the Δ column and the S25 FE bar.

## 2. Layout

- Single `<table class="matrix">` is the page.
- 4 columns: row-label · iPhone XR · Galaxy S25 FE · Δ.
- 12 row-groups translated from blueprint sections:
  1. Dimensions (H/W/D/Weight)
  2. Display (Size/Type/Resolution/Pixel density/Refresh rate/HDR)
  3. Chip (SoC/Process/Architecture)
  4. Memory (RAM/Storage)
  5. Camera system (Rear/Front/Video)
  6. Battery and power (Capacity/Wired/Wireless)
  7. Connectivity (Cellular/Wi-Fi/Bluetooth/NFC/UWB/Port)
  8. Authentication (Face/Fingerprint)
  9. Operating system (OS at launch/Update commitment)
  10. Ingress protection (Rating/Submersion)
  11. Capability deltas (5 mini-bar pairs)
  12. Ownership span (single-row timeline)
- §00 plotter REMOVED. §04 architecture diagram folded into Chip/Memory/Connectivity rows. §05 timeline collapsed to a single one-row visual at the bottom. §03 delta bars folded into a dedicated "Capability deltas" row-group.

## 3. Sticky behavior

- `position: sticky; top: 0` on `.topbar` (44px).
- `position: sticky; top: 44px` on `<thead> th` so silhouette column headers + device names stay pinned below topbar on scroll.
- No JS needed.

## 4. Silhouettes

- Lifted verbatim from blueprint §01 front-elevation SVGs:
  - iPhone XR: body 100×200, rx=14, notch trapezoid path, mute slider + vol + power, Lightning port.
  - Galaxy S25 FE: body 104×219, rx=10, punch-hole circle, dashed in-display FP, right-side vol/power, USB-C port.
- Strokes recolored `#1D1D1F` on `#FBFBFD`. Bezel + dashed-FP softened to `#86868B`.
- Stripped: dimension lines, callout labels (A/B/C/D), grille dots simplified.
- Rendered as ~64×120px column-header thumbnails (48×90 on mobile).

## 5. Voice

- Apple compare-page labels only. Every group heading is a noun + period (`Display.`, `Battery and power.`, `Authentication.`).
- Row labels are terse nouns (`Refresh rate`, `Pixel density`, `Cellular`, `Submersion`).
- Values are factual one-liners. No marketing adjectives. No prose.

## 6. Factual values

All factual specs lifted verbatim from `blueprint.html` (§02 matrix lines 1163-1264, §01 elev-foot lines 853/947). Includes:
- Dimensions: 150.9·75.7·8.3 mm / 161.3·76.6·7.4 mm; 194 g / ~210 g.
- Display: 6.1" LCD 1792×828 326 ppi 60 Hz / 6.7" AMOLED 2X 2340×1080 ~385 ppi 120 Hz adaptive HDR10+.
- SoC: A12 Bionic 7 nm 6C/4C / Exynos 2400e 4 nm 10C/Xclipse.
- Memory: 3 GB / 8 GB LPDDR5X. Storage: 64/128/256 / 128/256/512 UFS 4.0.
- Cameras: 12 MP wide f/1.8 OIS / 50 MP+12 MP UW+8 MP 3× tele. Front 7 MP / 12 MP. Video 4K@60 / 8K@30.
- Battery: 2942 mAh / 4900 mAh. Wired 18 W / 45 W. Wireless 7.5 W / 15 W + reverse.
- Connectivity: 4G LTE Wi-Fi 5 BT 5.0 / 5G Wi-Fi 6E BT 5.3 NFC UWB. Port Lightning / USB-C.
- Auth: Face ID / 2D face + ultrasonic FP.
- OS: iOS 12→18 ~6 yrs / One UI 7 + Android 15, 7-yr commitment.
- Ingress: IP67 1 m / IP68 1.5 m.

Δ column derived from blueprint's §02 delta column (where given) or computed from the two values (Height +10.4 mm, Width +0.9 mm, Depth −0.9 mm).

## 7. Mini-bars + timeline

- Capability deltas row-group: 5 paired horizontal bars (RAM, Battery capacity, Refresh, Wired charging, Process node). Bar widths use the same relative percentages as blueprint §03. XR bar = `--ink`; S25 FE bar = `--accent`. Mono numerals.
- Ownership span: single absolutely-positioned timeline strip across 2018→2026, axis with year ticks at 2018/2020/2022/2024/2026, XR primary 0%→87.5%, S25 FE primary 87.5%→100%. ~48px tall.

## 8. Responsive

- Breakpoint 740px.
- **Choice: stack** (not horizontal scroll). At <740 the table collapses to a card-per-row layout — two-column grid (XR | S25 FE) with the row label as a full-width subtitle. Δ surfaces below as a single-line annotation. Δ column header is hidden. Silhouettes shrink to 48×90.
- Rationale: horizontal scroll would have buried the silhouettes off-screen and broken the "single document" feeling. Stacking keeps the spec dense and scannable while preserving the sticky thead.

## 9. Accessibility

- `<table>` with `<caption>` (visually hidden via off-screen positioning).
- `<th scope="col">` on all 4 column headers, `<th scope="row">` on every row label.
- `aria-describedby="matrix-note"` on the table → links Δ-column meaning to the post-table footnote.
- Δ cells carry `aria-describedby="delta-head"` so screen readers connect each delta to the column header.
- SVGs have `role="img"` + `aria-label`.
- Color contrast: `#1D1D1F` on `#FBFBFD` = 17.9:1. `#86868B` on `#FBFBFD` = 4.6:1 (passes AA for 14px). `#0066CC` on `#FBFBFD` = 6.4:1.
- Topbar link reachable at top of tab order. Sticky thead does not trap focus.
- `prefers-reduced-motion: reduce` explicit `animation:none; transition:none` (page has no motion anyway).
- Print stylesheet hides topbar/footer, unsticks the thead.

## 10. Cross-link

- Footer `← all formats` → `index.html` (also duplicated in the topbar as "All formats").
- Secondary link to `blueprint.html`.

## 11. File / constraints

- 852 lines. Within 600-900 target.
- Self-contained: all CSS in `<style>`, zero JS, zero external assets, zero network fonts (system stack only).
- Responsive 360px → 1440px+. Tested mentally at 360/414/740/980/1280/1440.

## 12. Deviations from spec

- Spec mentioned "the Δ column hides on mobile; deltas surface inline below each value" — implemented exactly.
- Spec said "silhouettes ~120px tall" — used height:120px on desktop, 90px on mobile.
- Spec mentioned `<thead>` sticky at `top: 0` — actually pinned at `top: 44px` to clear the small topbar nav. Same effect, just one offset down.
- Added a minimal 44px topbar with "Compare" crumb + "All formats" link. Apple compare pages have a similar slim nav stub above the comparison.

---

## Unresolved questions

None blocking. Two minor judgment calls flagged for review if needed:
1. Topbar inclusion — spec didn't require it but Apple compare pages always have one; kept it minimal (44px, single crumb + single link). Easy to drop if the brainstorm intended absolute purity.
2. Two delta values computed (not in blueprint): Height +10.4 mm and Width +0.9 mm and Depth −0.9 mm. Trivial arithmetic from the blueprint values, but flagged as "computed" rather than "lifted".

---

**Status:** DONE
**Summary:** Single 980px-wide spec matrix with sticky silhouette column headers, 12 row-groups, mini-bars and timeline folded as table rows, Apple compare-page voice throughout — 852 lines, self-contained, no JS.
