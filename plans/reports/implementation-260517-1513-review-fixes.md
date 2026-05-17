# Implementation Report — Consolidated Review Fixes

**Date:** 2026-05-17
**File touched:** `/config/workspace/tiennm99/xr2s25fe/index.html` (single file)
**Source reviews:**
- `plans/reports/review-260517-1513-code-quality.md`
- `plans/reports/review-260517-1513-content-quality.md`
- `plans/reports/review-260517-1513-design-quality.md`

---

## Fix list — done / skipped

### MUST

| # | Fix | Status | Lines | Notes |
|---|-----|--------|-------|-------|
| 1 | §04 architecture responsive overflow | DONE | 537-558 (CSS), 1407-1408 (markup wrap), 1530-1531 (close) | Mirrored §05 timeline pattern: added `.arch-svg-wrap { min-width: 860px }` inside `.arch-wrap { overflow-x: auto }`. Added faint right-edge scroll-hint via `::after` linear-gradient under 900px viewport. |
| 2 | `.replot-replay` focus ring | DONE | 272-286 | Split hover and focus-visible into separate rules. Restored visible `outline: 2px solid var(--ink); outline-offset: 2px` on focus-visible. Added `@media (forced-colors: active)` block using `CanvasText`. |
| 3 | `.replot-replay` touch target ≥44×44 | DONE | 264-267 | `padding: 12px 16px; min-height: 44px; min-width: 44px`. Visual chip size unchanged — only hit area inflated. |

### SHOULD

| # | Fix | Status | Lines | Notes |
|---|-----|--------|-------|-------|
| 4 | §00 stagger amplification | DONE | 315-318 | Bumped `--stagger` tier values from 0/0.15/0.3/0.45 → 0/0.6/1.2/1.8s. Tier-3 finishes drawing at t≈5.825s, ~0.225s after Unit B begins (5.6s) — within reviewer's stated 2.2s limit. |
| 5 | Hero subtitle trim ≤50 words | DONE | 671 | Original 47 words (note: review claimed 105 — actual count was lower but still verbose). Trimmed to ~33 words. Voice preserved: "Side-by-side datasheet · iPhone XR ... No editorial, no score." This also absorbs Fix #10 ("no marketing surface" → "no editorial, no score"). |
| 6 | `--ink-3` contrast bump | DONE | 21 | `#7A7972` → `#6E6D67`. |
| 7 | Reduced-motion CSS unification | DONE | 384-393 | Converted `.replot.reduced-motion .rp-unit-b *` to explicit-list form matching the `@media (prefers-reduced-motion)` block above (path/line/rect/circle/polyline). |
| 8 | `<desc>` after each elevation `<title>` | DONE | 800, 905, 996, 1093 (approx, lines shifted) | Added `<desc>` with engineering-voice descriptions to all 4 elevation SVGs (XR front, S25 FE front, XR rear, S25 FE rear). Each `aria-labelledby` updated to reference both `title` and `desc` IDs. |
| 9 | Timeline EU mandate label | DONE | 1631-1633 | Both text and dashed line moved from x=469 (2021) to x=612 (2022) — matches Oct 2022 EU directive. No collision with iOS 16 marker (different y-bands: y=40 for contextual beats, y=100 for device band). |

### LOW / NIT

| # | Fix | Status | Lines | Notes |
|---|-----|--------|-------|-------|
| 10 | "no marketing surface" → "no editorial, no score" | DONE | 671 | Folded into Fix #5 subtitle rewrite. |
| 11 | "WALLED GARDEN" → "CLOSED PLATFORM" | DONE | 1417, 1528 | Both the column header (UNIT A · 2018) and the bottom DIST migration label updated for parallelism with "OPEN ANDROID" / "OPEN". |
| 12 | §02 add display ppi row | DONE | 1230-1235 | New row inserted after Display row: XR 326 ppi, S25 FE ~385 ppi, Δ "+59 ppi · +18 %". |
| 13 | §01 callouts add `mm` units | DONE (partial) | 1041, 1143 | Added "mm" to the two bare dim labels: `≈ 22` → `≈ 22 mm` (XR rear island) and `≈ 40` → `≈ 40 mm` (S25 ring-spacing). Other callouts (e.g. "C · ISLAND · 22 × 22 mm") already have mm or are descriptive — no change needed. |
| 14 | `data-label` attrs for mobile matrix | DONE | 446-449 (CSS), all `.col-a`/`.col-b`/`.delta` td (15 each) | CSS collapsed three `::before` rules into one using `content: attr(data-label)`. Added `data-label="XR — "`, `data-label="S25 FE — "`, `data-label="Δ "` to all 15 rows × 3 columns. iOS VoiceOver no longer relies on CSS-generated content strings. |
| 15 | Footer `<h2>` semantic check | SKIPPED | 1644 | "Document control" is a real landmark heading for the footer — kept `<h2>` semantics. Visual styling via `.micro` already matches the engineering-doc voice. Judgment call documented per task spec. |

---

## Tradeoffs / decisions

1. **§00 tier-3 stagger overlap.** With 1.8s stagger, tier-3 draw completes at t≈5.825s, just after Unit B begins at t=5.6s. The reviewer explicitly allowed up to 2.2s, so 1.8s is within spec. The visual effect: Unit B's tier-0 outline starts drawing while Unit A's tier-3 small details are finishing — that's the desired cross-fade hand-off, not a defect.

2. **§04 scroll-hint placement.** The gradient `::after` is anchored to `.arch-wrap` (the scrolling container) and gated on `@media (max-width: 900px)` to avoid visual noise on wide viewports. Single steel-blue colorway preserved — gradient is paper-to-transparent only.

3. **`data-label` text matches old `::before` strings 1:1.** Spacing and em-dashes preserved exactly so visual output is identical. Improvement is in robustness (real attribute vs. CSS-generated content) and accessibility on iOS VoiceOver.

4. **`<desc>` voice.** Kept measured engineering style — "Front orthographic elevation of …", listed edges by compass direction, used full spec phrases ("with OIS", "f/1.8 aperture"). No marketing adjectives.

5. **Hero subtitle word count.** Original was actually ~47 words, not 105 as the design reviewer estimated. New version is ~33 words. Removed list redundancy ("model-years … mapped as … display, silicon, …" had two lists; collapsed to one).

6. **Forced-colors override.** Added a separate `@media (forced-colors: active)` rule rather than overloading the main focus-visible. This lets Windows High Contrast paint a visible ring even when custom colors are stripped.

---

## Validation against aesthetic constraints

- No new colors introduced. `#FAFAF7` / `#1A1A1A` / `#3D5A6C` palette intact. `--ink-3` darkened within the existing neutral ramp.
- No gradients except the single linear paper-fade scroll-hint on `.arch-wrap::after` (a structural affordance, not decoration).
- No neon, LED, RGB, or glass-morphism added.
- All voice text remains engineering / documentation register.
- File still self-contained: no new external resources.

## Validation checklist (per task)

- [x] All visible text retains engineering-doc voice.
- [x] §04 horizontally scrolls below ~900px instead of crushing labels.
- [x] Tab to replay button → visible 2px ink outline, 2px offset.
- [x] §00 outline (tier 0) starts at t=0, details cascade through t=0.6/1.2/1.8s.
- [x] Page works without JS (CSS-only fallback preserved — script only sets `is-playing` class).
- [x] `prefers-reduced-motion: reduce` still hides Unit A and shows Unit B drawn.
- [x] No new colors.
- [x] File self-contained.

---

## Unresolved questions

1. **Stagger vs. cycle alignment.** With tier-3 at 1.8s, the last Unit A details finish drawing 0.225s after Unit B starts. Cleaner pacing would be tier-3 at ~1.5s to land exactly when Unit B begins, but the reviewer's stated max was 2.2s — sticking to the spec. Worth user gut-check on whether 1.5s feels better than 1.8s in motion.
2. **Display "ppi" row vs. Display row redundancy.** The Display row already mentions "326 ppi" for XR. New "Pixel density" row repeats it. Could either (a) keep both for at-a-glance scanning, or (b) strip ppi from the Display row to avoid duplication. Current state: kept both per fix #12 instruction. Flag for trim-pass.
3. **`<desc>` length.** ~60-80 words each. Screen readers announce title + desc — verify on a real reader pass that this isn't too verbose for keyboard users tabbing through 4 elevation SVGs.

---

**Status:** DONE
**Summary:** All 15 fixes applied to `index.html` (14 implemented, 1 explicitly skipped per judgment call). Aesthetic constraints preserved.
