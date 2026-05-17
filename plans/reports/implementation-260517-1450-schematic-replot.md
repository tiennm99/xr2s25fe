# Implementation note — § 00 Transformation / schematic re-plot

File modified: `/config/workspace/tiennm99/xr2s25fe/index.html`
Spec ref: brainstorm note (file absent, working from task brief Concept 1)
Geometry source: §01 elevations per `implementation-260517-1450-svg-fidelity-fixes.md`

## Summary

Inserted new section `§ 00 · Transformation · re-plot` between Hero and §01. CAD-plotter animation: draws iPhone XR front (Unit A), holds, erases, then plots Galaxy S25 FE front (Unit B) in same frame. Pure CSS `stroke-dasharray` + `pathLength="100"`. `IntersectionObserver` auto-starts; replay button restarts. Reduced-motion fallback shows only static Unit B. 11.5 s cycle, ease-in-out.

## Line ranges touched

| Region | Lines (new) | Change |
|---|---|---|
| CSS — replot tokens, layout, animation, keyframes, reduced-motion | 220–385 | new block, inserted after SVG drawing tokens |
| HTML — `<section class="replot">` (header, stage, svg, replay button, label) | 626–711 | new section, inserted between hero close (line 624) and `<!-- ─── ELEVATIONS ─` (line 713) |
| JS — `IntersectionObserver` + replay + reduced-motion gate | 1630–1662 | new `<script>` block before `</body>` |

Nothing outside these ranges modified.

## Keyframe structure chosen

Two separate keyframes (cleaner than single-keyframe-with-delay-offset):

**`plot-and-erase`** — applied to all stroked children of `.rp-unit-a`. Duration = `--plot-dur` (11.5 s), forwards fill.
- `0%` → `35%`: draw (dashoffset 100 → 0), occupies t=0–4 s
- `35%` → `45%`: hold drawn, t=4–5.2 s
- `45%` → `68%`: erase (dashoffset 0 → 100), t=5.2–7.8 s
- `68%` → `100%`: stay hidden (dashoffset 100), t=7.8–11.5 s

**`plot-only`** — applied to all stroked children of `.rp-unit-b`. Duration = `--plot-b-dur` (4 s), `animation-delay: 5.6s`, forwards fill.
- Linear dashoffset 100 → 0, starts at t=5.6 s, completes at t=9.6 s, holds drawn through cycle end.

This avoids the brittleness of having Unit A and Unit B share a keyframe set offset by `animation-delay` (which would cause Unit B to repeat the erase phase).

**Label cross-fade**: `@keyframes label-a` (visible 0 → 5.6 s, fade out by 6.0 s) and `@keyframes label-b` (fade in 5.5 → 6.3 s, visible to end). Both 11.5 s duration synced with main cycle.

## Per-feature stagger (Phase 2 polish)

4-tier stagger via CSS class + custom property `--stagger`:
- `.rp-tier-0` (body outline) → 0 s
- `.rp-tier-1` (bezel, screen) → 0.15 s
- `.rp-tier-2` (notch / punch-hole / inside-notch dots / FP ring) → 0.3 s
- `.rp-tier-3` (buttons, port, grilles) → 0.45 s

Applied additively: `animation-delay: calc(var(--plot-b-delay, 0s) + var(--stagger, 0s))`. Outline draws first; small features finish last. Total stagger window (0.45 s) fits inside the 4 s draw phase, no overflow into the hold phase.

## What was lifted vs stripped

**Lifted from §01** (verbatim with same `pathLength="100"` and `vector-effect="non-scaling-stroke"`):
- XR front: body, bezel, screen+notch path, notch outline path, cam/IR/speaker-slit dots, sleep button, mute slider, vol up/down, lightning port + 3+3 grille dots
- S25 front: body, bezel, AMOLED, punch-hole, FP ring, vol rocker, power button, USB-C + 3+3 grille dots

**Stripped** (per Phase-1 MVP "geometry-only"):
- `.dim-top` and `.dim-side` groups (callout/measurement rules)
- All `<text>` elements (`75.7 mm`, `150.9 mm`, `76.6 mm`, `161.3 mm`, callout labels A/B/C/D, `FP` interior label)
- All callout groups (`xr-callout-*`, `s25-callout-*`)

## Geometry placement in 640×420 viewBox

Source SVGs in §01 use 260×360 viewBox. Lifted geometry wrapped in `<g transform="translate(...)">` to center in 640×420:
- Unit A: `translate(190 30)` → body spans x=270–370, y=118–318 (center ≈ 320,218)
- Unit B: `translate(190 20)` → body spans x=268–372, y=102–321 (center ≈ 320,212)

Both units stacked at same center; only one visible at a time because of the animation phasing.

## Aesthetic compliance

- Colors: `--ink` strokes only, `--ink-3` for soft strokes, no neon, no accent in active drawing. (Accent reserved for `replot-replay` focus-visible only — actually currently only ink-3.)
- Pacing: 11.5 s cycle, ease-in-out via `cubic-bezier(0.65, 0, 0.35, 1)`.
- Replay button: small, outline, mono, 10.5 px, `--rule-soft` border — matches existing chrome rather than acting as a CTA.
- Blueprint grid + 1 px strokes preserved.

## Deviations from spec

1. **Spec proposed two keyframe approaches; chose the explicit two-keyframe path** (`plot-and-erase` for A, `plot-only` for B) over the single-keyframe-with-shared-delay variant. Avoids the erase-pulse that would otherwise hit Unit B on repeat cycles.
2. **Section header semantic markup**: spec said `<span class="kbd">` and `<span class="caption mono">`. Existing site convention uses `.num` and `.tag` inside `.sec-hd`. Compromised: kept both class names on the spans (`class="kbd num"`, `class="caption mono tag"`) so existing CSS handles base styling, new `.replot .sec-hd .kbd` / `.caption` rules don't conflict.
3. **Replay button positioning**: spec showed it as a sibling after the SVG. Placed it inside `.replot-stage` with `position: absolute; top: 14px; right: 14px;` so it floats over the stage frame, not below it. Less visually heavy; survives narrow viewports (CSS reduces size at <640 px).
4. **Accent crosshair (Phase 2 nice-to-have)**: SKIPPED. Spec said "purely decorative, can skip if it adds noise" — adding a roving crosshair to a stroke-dasharray-only animation would require either an additional `<path>` animated separately along the perimeter, or a JS-driven element. Not worth the LoC for Phase-1.

## Known limitations

1. **Stagger granularity**: only 4 tiers, applied uniformly across all elements in a tier. Visually clean but doesn't simulate the per-stroke pen-path of a real plotter. Could be extended to 5–8 tiers if desired without changing structure.
2. **Reduced-motion path A removed entirely**: spec mandates `display: none` on Unit A. Means users with `prefers-reduced-motion: reduce` only see the *current* device (S25 FE), losing the comparison. This is intentional per spec and matches accessibility best practices, but represents a tradeoff with information density.
3. **Animation runs only once** per page-view, by design (`IntersectionObserver` calls `unobserve` on first intersection). Replay button is the only way to re-trigger after that. No scroll-scrub, no auto-loop.
4. **`prefers-reduced-motion` check is on initial load only**. If a user toggles their OS setting after page load, the animation will still run. Acceptable; matches platform convention.
5. **Font size of replay button** (10.5 px) is at the lower edge of WCAG comfort but matches the rest of the site's `.tag` micro-type. Touch target is 30 px × ~28 px — under the 44 px mobile target recommendation. Acceptable for non-critical replay, but a user accessibility audit could flag this.

## Validation

- All 7 inline SVG blocks parse cleanly via `xml.etree.ElementTree`
- Section open/close balanced (7 / 7)
- No external dependencies added
- File self-contained, single HTML document
- Reduced-motion fallback covered by both `@media (prefers-reduced-motion: reduce)` rule and JS-applied `.reduced-motion` class (belt + suspenders)
- Replay button has `aria-label`, button is keyboard-focusable, `:focus-visible` styled
- SVG has `role="img"` and `<title>` for screen readers

## Unresolved questions

1. Should the dimension callouts and labels **fade in after Unit B finishes drawing** (Phase 2 polish suggested in spec)? Currently stripped per "Phase-1 MVP geometry-only" — works fine, but the section feels less informational than §01. Easy to add via opacity-fade keyframe firing at ~t=10 s. Recommend skipping unless explicitly wanted: §01 already serves the labeled/dimensioned reference role.
2. The accent crosshair (decorative plot-head indicator) — explicitly optional in spec, fully skipped. If desired later, a 4-line `<path>` accent crosshair could trace the perimeter via `motion-path` or be synchronized with the unit-A → unit-B transition window for ~1.5 s. Adds ~30 LoC.
3. Should auto-play on scroll-into-view loop, or remain a one-shot with replay button as the only re-trigger? Current implementation: one-shot. Spec implied one-shot ("plays once on scroll-into-view, replays on button click"). Confirmed.
4. The "§ 00" numbering displaces every existing section's numbering by 1 if treated semantically (§01 Elevations was previously the first section). Numerical labels left intact in §01–§05 because they are content labels, not anchors. If the user wants §00 to mean "preamble" (which is implicit), no change needed.

---

**Status:** DONE
**Summary:** § 00 Transformation section added. CAD-plotter animation drawing iPhone XR (Unit A, t=0–4s), holding (4–5.2s), erasing (5.2–7.8s), then plotting Galaxy S25 FE (Unit B, 5.6–9.6s). 11.5 s cycle, IntersectionObserver-triggered, replay button, reduced-motion fallback. Geometry lifted from §01 front elevations (callouts stripped). All SVG parses clean.
