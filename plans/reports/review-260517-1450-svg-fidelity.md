# SVG fidelity audit — §01 Elevations

File audited: `/config/workspace/tiennm99/xr2s25fe/index.html` lines 461–591
Reference targets: iPhone XR (Apple 2018), Galaxy S25 FE (Samsung 2025)

---

## Summary

Severity: **HIGH**. The renderings have the right *intent* (blueprint front elevation, mono ink, dim lines, callouts) but multiple hard-fact errors that an engineer would catch in 2 seconds.

Top offenders:
1. **Both phone bodies are far too tall.** Drawn ~3.5:1, real ratio ~2.0:1. They read as "tall rectangles," not phones.
2. **XR front elevation has a rear camera ring drawn on it** (line 499–500). Wrong side of the device. Comment even admits it: `<!-- camera bump (rear hint) -->`.
3. **XR notch is drawn as a tiny inward bump (8 px wide)** — real iPhone XR notch is wide (~⅓ of display width), trapezoidal-rounded, hangs from the top. Currently looks like a screen defect, not a notch.
4. **S25 FE punch-hole position is too far down** (cy=93 inside screen-top=82, only 11px down — looks fine relatively, but combined with the wrong-aspect body it sits weirdly low).
5. **No rear elevations exist** for either device. The §01 title says "front view" so this is technically in-scope, but the §02 matrix and §03 deltas heavily discuss 1-lens vs 3-lens camera systems — the rear is the most distinctive industrial-design difference between these two phones and it's not drawn anywhere. Strong recommendation: add a rear elevation per phone.
6. **Side buttons are wrong-placed and wrong-coded** on both. XR mute switch missing entirely; S25 has buttons on both sides (S25 family is right-side only).

Verdict: fix proportions first (single biggest visual lie), then fix the XR notch shape, then remove the rogue rear-cam from the XR front, then add proper rear elevations. Everything else is polish.

---

## iPhone XR — front (lines 471–524)

### Correct
- Title text "iPhone XR front elevation, 150.9 × 75.7 mm" — correct numbers (line 472).
- Top dimension `75.7 mm` label and side dimension `150.9 mm` label — correct (line 479, 486).
- Lightning port at bottom center (line 522) — correct port type & location.
- "LIGHTNING" caption (line 523) — correct.
- Generous corner radius `rx=14` on body (line 489) — directionally correct for XR aluminum frame (XR has noticeably rounder corners than S25-class flat-rail phones), though see proportions below.
- Single front-cam concept (line 495–497) — XR has 1 front-facing TrueDepth cluster, intent right.
- Footer specs: 150.9 · 75.7 · 8.3, 194 g, IP67 (lines 527–529) — all correct.

### Incorrect
- **Body aspect ratio (line 489):** `width="100" height="352"` → 3.52:1. Real XR is 150.9/75.7 = **1.994:1**. Body is ~75% too tall relative to width. Should be `width="100" height="200"` (or scale both axes — see fix list).
- **Notch shape (line 493):** the path `M 92 92 L 116 92 L 116 100 Q 116 106 122 106 L 138 106 Q 144 106 144 100 L 144 92 L 168 92 L 168 416 L 92 416 Z` produces a tiny center dip ~22px wide and only 8px deep, inside an 88px-wide screen area. Real XR notch is ~⅓ of display width and visually unmistakable. The current sketch is invisible at any reasonable display size.
- **Notch is below the bezel, drawn as part of the screen-area path.** Real iPhone notch *extends downward into the active display from the top bezel edge*, with the speaker/cam visible *inside* the notch cavity. The current geometry kinda matches but the notch is far too narrow.
- **Rogue rear-cam drawn on front face (lines 499–500):** `<circle class="drw-soft" cx="100" cy="108" r="6"/>` plus inner circle. This is a rear-camera bump rendered top-left on the FRONT elevation. Comment confirms: "camera bump (rear hint)". Either delete from front or move to a proper rear elevation.
- **Callout B (line 510) "B · 12 MP f/1.8"** points to the rogue rear-cam circle. 12 MP f/1.8 is the *rear* main camera, not front. On a front elevation this label is doubly wrong.
- **Side button graphics (lines 517–520):** lines drawn on body edges, but:
  - The right-side line at x=180 y=160→200 is one combined line — should be **two** separate lines (sleep/wake button on right side of XR is one short line; that part is roughly OK).
  - Left side has three short dashes representing volume up / volume down / something — but the **mute switch** (XR has a physical mute switch above volume rocker — distinctive iPhone signature) is not drawn or labeled. The three left-side marks need to be: mute switch (top, slightly different — could be a small rectangle slider), volume up, volume down.
  - All side buttons are drawn with `drw-soft` (faint) — they should probably match `drw` weight since they're a real geometric feature of the elevation outline.
- **Bezel/screen relationship:** `drw-soft` inner rect at x=86 y=84 w=88 h=340 (line 491) — implies a uniform 6px-equivalent bezel. XR has a noticeably thicker bottom "chin" bezel than its sides/top (it's symmetrical top-to-bottom — that's actually correct on XR, unlike Androids with chin). So uniform bezel is actually OK for XR. Keep.

### Missing
- No mute switch indicator on left side.
- No earpiece/speaker slit inside the notch (real XR has a thin slit speaker centered in the notch).
- No bottom speaker grille + mic grille either side of Lightning. Currently only the port is drawn.
- No Apple logo. (Front elevation: not needed. Rear elevation: would be needed.)
- No rear elevation at all.

---

## iPhone XR — rear

**Not drawn.** No rear elevation exists in §01. Given that §02 row "Rear camera" and §03 "Rear cameras · count" both highlight 1 vs 3 lenses as a key delta, the rear elevation is the most informative drawing missing from the doc.

Recommended additions for an XR rear elevation:
- Single square-rounded camera bump top-left (≈18×18 mm in real device, so ~9% of body width square).
- Inside the bump: 1 large circular lens (top) + 1 small circular flash (bottom-right of lens) + small mic hole. **Only ONE lens.**
- Centered Apple logo at ~⅓ from top.
- "iPhone" wordmark lower-third (subtle).
- Mute switch + volume on left edge (visible on rear elevation too as side-edge marks).

---

## Galaxy S25 FE — front (lines 538–583)

### Correct
- Title "Galaxy S25 FE front elevation, 161.3 × 76.6 mm" (line 539) — correct.
- Top + side dimensions 76.6 mm / 161.3 mm (lines 546, 553) — correct.
- **Center punch-hole** (line 562) — correct concept, correct hardware. Not a notch. ✓
- USB-C bottom center (line 573) — correct port type and location.
- "USB-C" caption (line 574) — correct.
- Under-display fingerprint sensor (line 564–565) — correct, S25 FE uses ultrasonic in-display FP.
- "ULTRASONIC FP" callout (line 580) — correct technology.
- Footer specs: 161.3 · 76.6 · 7.4, ~210 g, IP68 (lines 586–588) — correct.
- Bezel-less impression via tight inner rects (lines 558, 560) — correct intent (S25 FE has very thin uniform bezels).

### Incorrect
- **Body aspect ratio (line 556):** `width="104" height="374"` → 3.60:1. Real S25 FE is 161.3/76.6 = **2.106:1**. Body ~71% too tall. Should be `width="104" height="219"` (or rescale).
- **Corner radius:** rx=14 same as XR body. S25 family has tighter/sharper corner radius than iPhone XR's aluminum curve. Should be `rx=10` or `rx=11` to subtly differentiate from XR. (The radius is "less generous" on S2x flat-rail bodies.)
- **Side buttons drawn on BOTH sides (lines 568–570):** Real Galaxy S25 family has all hardware buttons on the RIGHT edge only (volume rocker above power button). The line at x=78 (left side) is wrong. Delete.
- **Right-side button geometry (lines 568–569):** `x1=182 y1=140 → y2=160` and `y1=170 → y2=200` — two segments for volume rocker + power, but they should be more visually distinct: a longer volume rocker (single thicker bar) sitting above a shorter power button, with a clear gap. Current drawing reads as just two equal lines.
- **No mic hole, no IR/flash indicator, no S Pen slot** — fine, S25 FE doesn't have S Pen, doesn't need extra holes on front. OK.
- **Punch-hole position (line 562):** `cy=93` inside screen y=82–436 (top of screen at 82). Vertical offset 11 px from screen top in a 354 px-tall screen reads OK proportionally (~3%), but on the real device the punch-hole is closer to ~4% from top — close enough, will look correct once body aspect is fixed.
- **Punch-hole size:** `r=2.4` against 88px-wide screen → ~5.5% screen width. Real punch-hole is ~3–4% screen width. Slightly oversized but tolerable for blueprint clarity.

### Missing
- No earpiece slit at the very top of the screen (S25 FE has a thin slit speaker in the top bezel above the punch-hole — though it's very subtle; not critical).
- No mute/Bixby alternate button — S25 FE doesn't have one, so correctly absent. Good.
- No SIM tray indicator on left edge — physical SIM still present on FE-line; optional detail.
- No rear elevation.

---

## Galaxy S25 FE — rear

**Not drawn.** And this is the most visually distinctive feature of the S25 family vs literally every other Android: **three discrete vertically-stacked camera rings sitting directly on the back glass** — NOT a clustered camera island like Pixel/Find/most others.

Recommended additions for an S25 FE rear elevation:
- Three separate raised circular rings stacked vertically along the upper-left back, with small visible spacing between rings.
- Top ring = telephoto (slightly smaller), middle ring = wide/main (largest), bottom ring = ultrawide (medium). Each ring should have a center circle (the lens) and a thin outer ring (the metal bezel that rises from the back panel).
- Small flash + mic dot to the right of the rings (typically near the top or beside the middle lens).
- "SAMSUNG" wordmark lower-third or lower-bottom (subtle, smaller text — real S25 FE has very minimal wordmark).
- The three rings are unique because they have NO surrounding plateau / camera-island rectangle. This is the S2x/S25 signature — get this detail right and the drawing instantly reads as "Galaxy S".

---

## Side profile / dimensions / aspect ratio

**No side-profile elevation drawn.** Only front. Real industrial differences between the two side profiles are:

- **XR:** aluminum rounded frame, ~8.3 mm thick, rails curve into front + back glass smoothly (classic iPhone 6–11 "soap bar" profile).
- **S25 FE:** flat aluminum rails, ~7.4 mm thick, sharper transition between rail and glass — the "flat rail" look that started with iPhone 12 and that Samsung adopted on S22+.

If a side profile were added, it would show:
- XR slightly thicker (8.3 vs 7.4 mm).
- XR with curved/convex side rails.
- S25 FE with flat rails.

This is a strong visual contrast and worth adding if space allows.

**Aspect-ratio summary (CRITICAL):**

| Phone | Real ratio | Drawn ratio | Error |
|---|---|---|---|
| iPhone XR | 1.99:1 | 3.52:1 | +77% too tall |
| Galaxy S25 FE | 2.11:1 | 3.60:1 | +71% too tall |

Both bodies look like remote controls, not phones. Fix this first; everything else is cosmetic until proportions read true.

---

## Prioritized fix list (top = highest visual impact)

### 1. Fix body aspect ratios (CRITICAL)
- **Where:** XR body rect line 489 + nested bezel/screen rects lines 491, 493. S25 body rect line 556 + nested rects 558, 560.
- **Wrong:** XR 100×352, S25 104×374.
- **Should be:** Keep widths roughly the same so labels align, *reduce heights*.
  - XR: width 100, height ≈ 200 (ratio 2.0). Recenter vertically within viewBox 0 0 260 480.
  - S25 FE: width 104, height ≈ 219 (ratio 2.1).
- **Knock-on:** the side-dimension dashed lines (lines 482–485 XR; 549–552 S25), the screen/bezel rects, the notch path, the punch-hole circle cy, the FP circle cy, the side-button line y-coords, the port rect y, and the callout endpoints all need their y-coords scaled to the new body height.
- **Recommendation:** either rescale viewBox vertically (e.g., `viewBox="0 0 260 320"`) and let the page CSS shrink the cell, OR keep viewBox and shorten body and reflow inner geometry — first option is faster and preserves all label proportions.

### 2. Remove XR's rear-camera from the front face
- **Where:** lines 498–500 (`<!-- camera bump (rear hint) -->` plus two circles at cx=100 cy=108).
- **Wrong:** rear camera drawn on front elevation.
- **Should be:** delete those two circles + the callout that references them (lines 507–510: the `B · 12 MP f/1.8` callout line and text).
- Migrate that callout to a new rear-elevation drawing.

### 3. Redraw XR notch at correct scale and shape
- **Where:** line 493 path `M 92 92 ... Z`, plus notch-detail elements lines 495–497.
- **Wrong:** notch is ~22 px wide on an 88-px screen (~25%) but only 8 px deep, drawn as a tiny v-shaped dip.
- **Should be:** real XR notch is ~⅓ of display width, hangs ~6–7 mm into the display. On the SVG that means roughly 30 px wide × 8–10 px deep, with the notch being a flat-bottomed, rounded-corner trapezoidal pendant hanging from the top edge of the active display.
  - New path concept: from top-left of screen go right to (notch-start), drop down 8 px with a small rounded corner (rx≈3), travel right across the notch bottom (~30px), rise back up with same rounded corner, continue to top-right corner of screen, then down/around the rest of the screen rect.
  - Inside the notch: a small ellipse (speaker slit, ~10×1.5 px), a small circle for IR camera (left of speaker), a small circle for front camera (right of speaker), an even smaller dot for proximity sensor. Use `drw-soft` for these inner details.
- **Coords to update if body height changes:** all y-coords scale with the new body geometry.

### 4. Add Samsung S25 FE rear elevation (new SVG)
- **Where:** new `<article class="elev-cell">` inside the elev-grid, or a sub-row below current cells.
- **Content needed:**
  - Body rect, slightly tighter rx than XR (rx=10), correct aspect 2.1:1.
  - Three vertically stacked discrete camera rings on upper-left, each ring = outer circle (drw) + inner lens circle (drw-soft). Rough placement: ring centers at ~(x_body+18, y_body+25), (x_body+18, y_body+50), (x_body+18, y_body+75) with outer radii ~9 / 10 / 8 (telephoto smaller / wide largest / ultrawide medium). Spacing ~16 px between centers.
  - LED flash + mic: two small dots ~10 px to the right of the middle ring.
  - "SAMSUNG" text bottom-center, lbl-sm, very subtle.
  - Callouts for each lens: "A · 50 MP WIDE", "B · 12 MP ULTRAWIDE", "C · 8 MP 3× TELE".
- **Why high priority:** the 1-lens-vs-3-lens delta is the single most discussed spec in §02 and §03. Without a rear drawing the document misses its highest-value visual.

### 5. Add iPhone XR rear elevation (new SVG)
- **Where:** matching new article in elev-grid.
- **Content needed:**
  - Body rect, rx=14 (rounder than S25), correct aspect 2.0:1.
  - Single rounded-square camera bump top-left, ~22×22 px on an SVG body of ~100 wide. Rounded corners rx≈4.
  - Inside the bump: 1 lens (one large circle drw + small inner circle drw-soft), 1 flash (smaller circle, ~3 px), 1 mic hole (tiny dot). Vertically stacked or lens-top / flash-bottom.
  - Centered Apple logo placeholder: small circle or "" glyph at body-y ≈ ⅓ from top (or omit logo and just leave clean back — Apple style).
  - "iPhone" wordmark bottom-third — optional, very subtle.
  - Callout: "A · 12 MP WIDE f/1.8 · SINGLE LENS" — the SINGLE LENS phrase is the punchline.

### 6. Fix S25 FE side buttons
- **Where:** lines 568–570.
- **Wrong:** button drawn on left side (x=78 line 570). S25 family is right-side only.
- **Should be:** delete line 570. Keep right-side buttons (lines 568, 569) but consider drawing the volume rocker as one slightly longer single line (or a thin rect to suggest a rocker), and the power button below it as a shorter line with a small gap between them. Both should use `drw` (not drw-soft) since they're real geometry.

### 7. Fix XR side buttons (add mute switch)
- **Where:** lines 517–520.
- **Wrong:** mute switch missing; left-side has three undifferentiated short lines.
- **Should be:**
  - Left side, topmost: a small rectangle ~3×6 px representing the mute slider toggle (XR signature).
  - Below it: volume up (short line).
  - Gap.
  - Below: volume down (short line).
  - Use `drw` weight.
  - Right side: sleep/wake button — single short bar ~18–20 px tall, currently at y=160–200, possibly too long. Reduce to ~25 px. Keep position at right-rail (x=180 currently fine, adjusts when body shrinks).
- Label the mute switch in callout: "MUTE / RING".

### 8. Fix bottom-edge detail on XR + S25 FE
- **Where:** XR line 522 (Lightning), S25 line 573 (USB-C).
- **Missing:** speaker grille (right of port) + mic grille (left of port). Add 5–6 tiny dots or short lines either side of each port, using `drw-soft`. Same treatment on both devices for consistency.
- **Optional callout:** "SPK / MIC" pointing to the grille pattern.

### 9. Lighten or remove the small dots inside XR notch
- **Where:** lines 495–497.
- **Current:** two small circles + one rect. At current scale they're invisible. After the notch is enlarged per fix #3 these become meaningful — keep them but use lbl-sm-scale geometry.

### 10. Optional — add side profile mini-elevation
- **Where:** below each phone's front elevation or in a third row.
- **Content:** thin horizontal rectangle (length = body height, height = thickness). XR thicker (8.3 mm scaled), curved end caps. S25 FE thinner (7.4 mm scaled), flat ends with crisp 90° transition between rail and front/back.
- Adds significant info density (thickness + rail-profile delta) for minimal space.

---

## What to preserve (do NOT clobber on fix)

- All numeric labels (75.7 mm, 150.9 mm, 76.6 mm, 161.3 mm) — correct.
- Footer specs in both `.elev-foot` blocks — correct.
- Title text in `<title>` elements — correct.
- Dimension dashed-line technique using `drw-dash` class — correct blueprint convention.
- Callout dashed leader-lines + lbl-sm caption combo — correct, on-brand technical-drawing style.
- Port-type labels "LIGHTNING" and "USB-C" — correct, central to the cross-generational story.
- S25 FE has correct port (USB-C), correct front cam shape (punch-hole), correct under-display FP. These three are signature S25 features — keep.
- XR has correct port (Lightning), correct generous corner radius (relative to S25 — when both rx values are set differentially), correct LCD callout. Keep.
- §02 matrix and §03 deltas are not part of this audit but accurately describe the deltas the elevations should visualize — don't touch them.
- The viewBox dimensions `0 0 260 480` *can* stay if the inner body is rescaled; or change viewBox to `0 0 260 320` and rescale inner — either works.
- All `drw`, `drw-soft`, `drw-dash` class usage is appropriate — keep the visual language consistent in any new rear elevation SVGs.
- Color tokens: stay on ink + soft-ink + dashed-ink. No accent `--accent` should appear in elevations (verified — none used currently). Keep that restraint.

---

## Unresolved questions

1. **Add rear elevations inline, or as a separate §01b section?** Recommendation: add as two more `<article class="elev-cell">` blocks inside the same `.elev-grid` (so grid becomes 2×2: XR-front, S25-front, XR-rear, S25-rear). User decision.
2. **Should the side-profile mini-drawing be added?** It's high-info-density but adds a third row to §01. Not strictly required by the audit, but recommended.
3. **Keep aspect-fix conservative (just scale heights) or rescale the whole viewBox?** Recommendation: keep viewBox 260×480 *if* a side-profile or rear elevation is added below the front (uses the extra vertical space). Otherwise reduce viewBox to 260×320 to remove dead space.
4. **The XR notch fix needs to know the final body width** — the notch is a ~⅓-screen-width pendant. Coord guidance above assumes screen width stays at 88 px. If body width changes, recompute the notch path proportionally.
5. **Is the user OK with adding new SVG `<article>` cells, or should fixes be constrained to the existing two cells only?** If constrained, fixes 4 + 5 are dropped and the doc remains front-only.

---

**Status:** DONE
**Summary:** Audit complete; biggest issues are wrong aspect ratios on both bodies, a misplaced rear-camera circle on the XR front elevation, an undersized XR notch, and the complete absence of rear elevations where the most distinctive industrial-design deltas (1-lens vs 3-discrete-rings) live.
