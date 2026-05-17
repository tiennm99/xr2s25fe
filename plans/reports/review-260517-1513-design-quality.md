# Design Quality Review — `index.html`

**Method:** Source-read-only. Browser tooling (puppeteer / chromium) not installed in this sandbox; per global rules I did not auto-install. Static server was started at `:8765` and served fine, but no headless engine available. Findings come from a careful trace of `index.html` (1664 lines), mentally simulating render at 360 / 640 / 720 / 820 / 1024 / 1280+ widths. Anything I flag as **[unverified-visual]** is a claim I'd want to confirm against a real render before signing off on it.

---

## TL;DR

A genuinely accomplished single-file artifact. The blueprint metaphor is held with conviction — token system is tight, monochrome restraint is intact, type ladder is mature, and the §00 re-plot animation is the kind of "earned" centerpiece that justifies the page existing. Two real defects: **§04 Architecture is hostile below ~1000px** (no scroll fallback, no stack reflow, no min-width — 200px columns crush, 9–10px labels approach illegibility), and **focus rings are stripped without replacement** on the only interactive control. Otherwise the page reads as a coherent published artifact, not a hobby file.

| Axis | Score (1-5) |
|---|---|
| Hierarchy | 4.5 |
| Animation (§00) | 4 |
| Responsiveness | 3 |
| Brand fit / restraint | 5 |
| Accessibility | 3 |

---

## What works (preserve)

- **Token discipline** (L13–31): single grayscale ladder (`ink` → `ink-2` → `ink-3` → `rule-soft` → `rule-faint`) + one accent + two paper tones. No drift anywhere I could find. This is what makes the page feel published.
- **Title block as engineering frame** (L602–623): `tb-side` four-cell metadata grid borrowing the title-block convention from real engineering drawings. It immediately signals "this is a document, not a marketing page". The bottom-right `.scale-bar` (L607–615) is a confident visual flex.
- **`pathLength="100"` normalization** across 133 elements (L642–1102) — clean engineering choice that makes the §00 animation timing deterministic regardless of path geometry. Worth flagging as a small craft win.
- **Corner registration marks** (L572–579, fixed) — subtle (`opacity: 0.35`), pinned to viewport corners, hidden on mobile. Earns its space.
- **Section header strip** (L67–100): mono `§ NN` + sans heading + mono right tag. Three-column grid that collapses to single column on mobile. Repeated identically across all six sections. Rhythmic and unobtrusive.
- **Faint blueprint grid** at 32px × 32px (L43–48). Subtle enough to read as paper grain on first scroll, structural enough to reinforce the doc voice.
- **Mono for all numbers/units** rule is observed consistently — h1 arrow, callout text, delta values, footer metadata, dimension labels.
- **Hero h1** (L604, L140–147): `clamp(34px, 6vw, 64px)` with letter-spacing -0.025em and weight 500 — the typographic centerpiece of the page, restrained but properly heavy. Inline `//` and `→` as mono accent-blue glyphs (L148) is the single best small-detail decision on the page.
- **Matrix mobile reflow** (L437–450): full row-to-card collapse with `::before` pseudo-labels prefixing each value ("XR — ", "S25 FE — ", "Δ ") so context is never lost when the column headers disappear. Properly thought through.
- **Reduced-motion fallback** for §00 (L357–378): unit A hidden entirely, unit B rendered fully-drawn statically, replay button hidden, label A hidden. Graceful — reduced-motion users still see the "after" state, which is the actual information payload.
- **§00 SVG geometry is recycled from §01** (comments L639, L676) — the re-plot uses the exact same path data as the elevations. That self-consistency is what makes the animation feel diegetic to the document rather than tacked-on motion.

---

## What's weak (highest-impact problems)

1. **§04 Architecture has no responsive strategy.** `viewBox="0 0 1080 520"` set to `width: 100%` (L525). At 360px viewport, the left/right 200-px-wide blocks compress to ~67px wide, and the 9–10px `.lbl` text scales down to ~3–4px. The center "MIGRATION CHANNEL" with paired callout text (L1444–1484) becomes a wall of crushed glyphs. Compare to §05 Timeline (L548: `min-width: 720px`, with `.tl { overflow-x: auto }` at L546) which solves the same problem correctly. **§04 needs the same treatment.**
2. **`outline: none` on replay button** (L273) with only color / border swap as the focus indicator (L269–274). For sighted keyboard users this is a real regression — keyboard focus is functionally invisible on a 1px-soft-border → 1px-rule-color change unless the user is already looking at the button. Either restore a visible focus ring (a 1px accent or ink offset-outline would match the doc voice) or make the swap unambiguous (e.g. accent-color border + background-color tint).
3. **§00 stagger barely registers visually [unverified-visual].** All four tiers (L302–305: 0s / 0.15s / 0.3s / 0.45s) sit on top of an 11.5s cycle and a 4s per-stroke draw. Total tier-3 lag is 450ms — about 4% of the cycle, ~11% of a single draw. The "outline → details" choreography is conceptually there but the human eye may not parse it as deliberate sequencing. Either bump stagger to ~0.25s / 0.5s / 0.75s / 1s (cumulative 1s), or compress the per-stroke draw window so the stagger reads as deliberate beats.
4. **§01 elevation art width is capped at 260px** (L193). Inside a `.elev-cell` that on desktop is ~600px wide, that leaves ~340px of empty paper-2 around each silhouette. On a true 1440px viewport the elevations sit small and isolated. They could breathe — `max-width: 320px` or no cap would make the dimensional callouts much more legible without hurting the grid feel.
5. **Hero `.subt`** (L605) is 105 words / ~640 chars of dense prose. The doc voice tolerates terseness — this paragraph fights it. Cut by half (≤50 words). The crumb + h1 + title-block side panel already communicates the framing; the prose should add one sentence of intent, not summarize the whole sheet.
6. **README ↔ CSS color drift.** README L70 says paper `#F5F0E8`; CSS L14 says `#FAFAF7`. The locked aesthetic constraint matches CSS. The README is stale. Not a render bug but a documentation defect that will trip future maintainers.
7. **README ↔ matrix SoC drift.** README L17 says "Exynos 2500"; matrix row at L1145 says "Exynos 2400e". Pick one and align both.

---

## Animation review — §00 Transformation

Specifically:

- **Concept: 5/5.** A datasheet that *plots itself*, with iPhone XR drawn first, held, erased, then S25 FE plotted in the same frame. This *is* the story of the page. Re-plot as metaphor for upgrade. Cannot be improved at the conceptual level.
- **Placement: correct.** Sitting between Hero and §01 it functions as a moving title-page flourish that previews the silhouettes you're about to see annotated. Reordering it elsewhere (post-elevations, post-matrix) would lose that "this is what we're about to dimension" framing.
- **Cycle length 11.5s [unverified-visual].** Probably feels long-ish on first watch but appropriate given the strokes need to read as deliberate plotting. The "hold drawn" window (45% of cycle, ~5.2s) before erasing is generous — that's good; the eye needs time to read the XR before it disappears. The S25 FE plot starts at 5.6s and holds drawn from ~9.6s to end of cycle, then the cycle doesn't loop (`forwards` on the keyframes). **Verify:** does the section actually replay only via button after the first auto-trigger, or does it loop? Reading the CSS: `forwards` + `is-playing` class added once via `IntersectionObserver` (L1639–1648) with `obs.unobserve(section)` — confirms **single-play with manual replay**. Correct UX choice. Looping would be annoying.
- **Replay button** (L255–274, "↻ REPLAY"): top-right placement, mono caps, transparent → 1px-soft-border. Discoverable on desktop hover. **Touch target: too small.** Padding `5px 10px` + 10.5px font → roughly 24×16 px hit box. WCAG 2.5.5 Level AAA target is 44×44; minimum acceptable is 24×24. Borderline-failing on mobile. Bump padding to `10px 14px` and add `min-height: 32px` minimum, ideally larger.
- **Replay button styling matches doc voice** — good. It is not a CTA button, it's a doc affordance, and reads as one.
- **Reduced-motion fallback:** end-to-end correct (L357–378). Users see Unit B fully drawn, no replay button, label B static. They get the final state — which *is* the message of the section (the upgrade is the S25 FE). Well executed.
- **Stagger tiers are too small to read** — see "What's weak" item 3.
- **Cross-fade labels** (L342–354) at t=45→52% and t=48→55% is a 7% / 3.5% overlap. Probably reads as a clean hand-off. **[unverified-visual.]**
- **One missed beat:** the §00 caption tag says "click to replay" (L631). Replay button label says "↻ REPLAY". The arrow icon implies replay but doesn't say it; on mobile where hover doesn't exist, the affordance is purely visual. Consider adding `aria-label` only — actually it already has one (L635: `aria-label="Replay re-plot"`). OK.

**Animation verdict: teaches the story, doesn't decorate. Stagger needs amplification; touch target needs growing; otherwise the centerpiece of the page does its job.**

---

## Section-by-section findings

### Hero (L597–624)

- Title block executes the engineering-document convention well. `tb-main` left, `tb-side` 4×1 metadata grid right at `320px` (L115). Mobile reflow at <820px makes `tb-side` a 2×2 grid (L164–170), which preserves the title-block density rather than letting the metadata stretch wide.
- `h1` `clamp(34px, 6vw, 64px)` with line-height 1.02 (L140–147) — proper display type. Inline mono `//` and `→` in accent (L148) is a small craft signal that does heavy lifting for tone.
- `.scale-bar` (L155–162) at hero bottom with `SCALE 1 : 1` + `0 — 50 mm` ticks is a quiet flex that says "this is a drawing, not a webpage". One of the better small details.
- **Weak:** `.subt` is too long (see What's weak #5).
- **Weak:** `.crumb` (L129–138, L598–600) shows breadcrumb with `Rev A — 2026.05`. Same revision date is also in the title block (`2026-05-17`) and the footer. Three near-duplicate revision strings. Pick one to be authoritative; demote the others or remove the crumb's revision token.

### §00 Transformation (L627–711)

See dedicated animation review above.

Additional: replot-stage (L242–247) has only 1px border / paper background. No internal grid overlay. Compared to §04 architecture (also boxed) and §05 timeline (boxed with 36px padding), §00 feels slightly more spartan. That's fine — the SVG itself is the content. Don't add ornament.

### §01 Elevations (L714–1116)

- 2×2 grid (L173–180): XR-front · S25-front (row 1), XR-rear · S25-rear (row 2). **Pairing is intuitive** — front-to-front, rear-to-rear, comparison per axis.
- Mobile collapse to 1-column at <720px (L203–209) — clean, no broken cell borders **[unverified-visual]**.
- **Silhouettes [unverified-visual]:** XR with notch + slim symmetric bezels + Lightning port and mute slider on left should read as iPhone XR to anyone who saw an iPhone X-series. S25 FE with punch-hole + ultrasonic FP ring + right-side-only buttons + USB-C should read as a modern Galaxy. The rear views are where recognition becomes harder — XR rear shows a single 22×22mm camera island top-left, no Apple wordmark (placeholder circle), and a subtle "iPhone" text. S25 rear shows three discrete unringed lenses with SAMSUNG wordmark. The S25 rear callout "3 DISCRETE RINGS · NO ISLAND" (L1094) is a smart explicit cue precisely because the silhouette difference might not register without it.
- **Dimensional callouts** are useful at desktop scale. At mobile the SVG stays scaled but the `.lbl-sm` size (8px, L218) approaches the floor of legibility. **[unverified-visual.]**
- **Empty space** in each `.elev-cell` is significant (see What's weak #4) — the SVGs sit small relative to their padded container at desktop.
- **Cell footer trio** (L818–822, L911–915, etc.): `H × W × D` / `Mass` / `Ingress` in mono — proper data-sheet density.
- **Border math:** `.elev-cell:nth-child(2n)` removes right border, `:nth-last-child(-n+2)` removes bottom border (L182–183). Works at 2 columns; mobile reflow re-adds the bottom border (L205–207). Clean.

### §02 Spec Matrix (L1119–1223)

- 14 rows, 4 columns. Δ delta column on the right in accent steel-blue (L428–434). The Δ values are doing real work — "+5 GB · 2.67×" communicates magnitude better than the raw "3 → 8" alone.
- **Δ legibility: good** at desktop. The accent color provides scan-anchor at the right edge. On mobile cards (L437–450), Δ becomes "Δ " prefix on its own line — preserves the scanning behavior at small widths.
- **Row count: appropriate.** 14 is dense but feels exhaustive without padding. Every row clears the bar of "did this actually change?".
- Borderline-weak rows: `Weight +16 g · +8%` is a small delta in a sea of large ones. It's still real data but it's the row most likely to feel like "we had a slot to fill". Could keep — it's part of the doc voice ("document every dimension that changed") — or drop without harm.
- `Note · figures sourced from...` at the bottom (L1222) — appropriate footer caveat. Doc voice maintained.
- **Caveat I can't verify visually:** alternating row backgrounds aren't used; only `border-bottom: 1px solid rule-faint` separates rows (L407). 14 rows of identical paper background may feel monotone at scale. **[unverified-visual.]** Consider a very subtle `.matrix tbody tr:nth-child(even) { background: var(--paper-2) }` — but only if the render confirms monotony; if it reads as clean today, leave it.

### §03 Capability Deltas (L1226–1354)

- 8 bar cells in a 2×4 grid. All XR bars are shorter, all S25 FE bars are longer — direction is correct and consistent (L1239 37.5%, L1244 100%, etc.).
- **Bar % math sanity check:**
  - RAM: 3/8 = 37.5% ✓
  - Battery: 2942/4900 ≈ 60% ✓
  - Refresh: 60/120 = 50% ✓
  - Charging: 18/45 = 40% ✓
  - Cameras: 1/3 = 33.3% ✓
  - Network 4G→5G: 55% (judgment call, not numeric) — fine
  - Process 7→4nm: 57% (inverted ratio judgment — fine as "older / larger")
  - Main MP: 12/50 = 24% ✓

  All math is honest; no compressed or exaggerated bars. Good design integrity.
- Number labels (the "3 GB" / "8 GB" at left and "XR" / "S25 FE" at right, L485–496) in mono 12px — legible at desktop. Mobile **[unverified-visual]**: 1-column collapse at <720px (L512–517), bars stretch full width, labels stay 56px columns each side — should remain legible.
- **Δ Direction discoverability:** the `.factor` label in each cell head (L478–482) ("× 2.67", "+1958 mAh", "× 4.17", "4G → 5G") is the key piece — it tells the reader the magnitude before they parse the bars. Nicely done.
- **Bar count: focused.** 8 is the right number — same dimensions as the matrix but only the ones where a bar/factor adds something beyond the table. Good editorial discipline.

### §04 Architecture (L1357–1494)

- **Diagram concept: strong.** Two vertical stacks (UNIT A left, UNIT B right) with a centered "MIGRATION CHANNEL" boxed between them, paired by horizontal row: OS / SoC / MEM / AUTH / I/O / RF / DIST. Each row has small accent connector strokes from stack to channel (L1452–1453, L1457–1458, etc.). The legend (L1488–1492) with three keys (subsystem block / migration delta / logical link) clarifies the visual language.
- **At desktop ~1280px width, this is the strongest single diagram on the page.** A tech-literate stranger could follow the iOS-to-Android ecosystem story from this image alone.
- **Below ~1000–1100px, it falls apart.** See What's weak #1. There is no `min-width` + horizontal scroll like §05 has, and the SVG `width: 100%` (L525) just keeps shrinking. The stack labels (`OS · iOS 12 → 18` at ~9px) become illegible. **Top defect of the page.**
- **Fix is small:** either wrap `<svg>` in a `min-width: 1000px` + `overflow-x: auto` container (matches §05 pattern at L546–548), or progressive-enhance: reflow stacks vertically below a breakpoint (much more work, probably overkill). The min-width-scroll approach is the YAGNI/KISS answer.
- **Smaller:** the `arch-legend` (L526–539) renders the dashed-line swatch using `border-top: 1.5px dashed` on a 0-height span (L539). Cute hack, but at certain zoom levels the dashed marks may anti-alias differently than the actual dashed lines in the SVG. **[unverified-visual.]**

### §05 Timeline (L1497–1596)

- 2018→2025 strip, 8 year ticks, lifespan band in accent-tint (`#EEF1F3`, L24) running the full span, three "generational tech beats" above axis (5G ROLLOUT / USB-C MANDATE / ON-DEVICE LLMs), four milestone markers below (XR LAUNCH / iOS 16 / iOS 18 XR FINAL / S25 FE LAUNCH).
- **Density: appropriate.** Eight years across ~1000 SVG units leaves ~143px per year — enough room for milestone labels without cramping.
- **`min-width: 720px` + horizontal scroll** (L546–548) on the wrapper — *this is the right pattern §04 should adopt*.
- **Lifespan band vs milestone markers differentiate cleanly**: band is the accent-tint horizontal rectangle, markers are dashed vertical lines + filled circles on the axis. Two distinct visual languages. Reads correctly.
- **S25 FE launch marker uses accent color** (L1574–1575) — small visual cue that the right-end-of-timeline is "the new device", consistent with the §02 Δ-column and §03 factor labels treating accent as "new state".
- **Y-axis labels** ("DEVICE LIFESPAN BAND" L1544, "CONTEXTUAL BEATS" L1545) — left-aligned mono 8px. Probably blends into the background as soft chrome **[unverified-visual]**.

### Footer / doc-control (L1599–1627)

- Six-column metadata grid: Drawing no. / Revision / Sheet / Maintainer / Status / Notes. `<h2 id="ft" class="micro">Document control</h2>` (L1600).
- **Matches the hero title-block visually** — same mono+ink-3 K-V pair pattern (L568). The page opens and closes with the same engineering-document framing device. Strong coherence.
- **Mobile reflow** to 2 columns at <640px (L569) — clean.
- One nit: the title block uses uppercase keys ("Document", "Revision", "Span", "Sheet") in the *side* panel (L618–621) with title-case styling (L126: `text-transform: uppercase`). Footer keys ("Drawing no.", "Revision", "Sheet") are also uppercased via CSS (L565). Consistent. But the *values* in the title block are mono 13px (L127) while footer values are sans 12px non-uppercase (L568). Slight inconsistency — title-block values feel slightly heavier / more "engineered". The footer values look like prose by comparison. Consider making footer values mono too for full consistency, or accept the variation as "title block = stamped, footer = annotated".

---

## Responsive findings

| Width | Verdict |
|---|---|
| **1440px+** | Designed-for. Hero h1 hits ~64px cap; title-block side panel sits at 320px; elevation grid is 2×2 with breathing room. §04 architecture renders at full readable density. Likely the page's intended showcase width. |
| **1280px** | Same layout, slightly tighter. Frame max-width is 1280 (L30) so 1280+ is the canonical canvas. |
| **1024px** | Still 2×2 elevation grid, still 2×4 delta grid. §04 starting to feel cramped — block labels approach 11px equivalent. Beginning of trouble zone. |
| **820px** | Title block collapses (L164–170): main panel becomes full-width above, side panel becomes 2×2 below. Reflow works. §04 worse — see #1. |
| **720px** | Matrix collapses to card rows (L437); elevation grid → 1 column (L203); delta grid → 1 column (L512). All three handled correctly. Timeline kicks in `overflow-x: auto`. **§04 has no breakpoint and is now broken — main responsive defect.** |
| **640px** | Frame padding tightens to 18px (L57–60); section padding shrinks 72→48px (L64); section header sec-hd collapses to single column, drops the right tag (L97–100); §00 caption hidden (L240); §00 replay button shrinks (L380–383); registration marks hidden (L580). All deliberate. |
| **360px** | Body font 14px (L59). h1 reaches `clamp(34px, 6vw, 64px)` → 34px floor (acceptable). Title block 1-col stacked. Matrix card rows readable. **§04 disastrous** at this width — entire diagram crushed to ~324px (360 - 2×18 padding), 200-px-wide stack blocks compress to ~66px, 9px text scales to ~3px. |

**Responsive verdict:** §02, §03, §05, hero, elevations, footer all reflow correctly. §04 is the single neglected breakpoint. Fix once and the responsive story becomes clean across the board.

---

## Visual accessibility findings

- **Contrast (paper `#FAFAF7` vs):**
  - `--ink #1A1A1A`: ~16.7:1 — passes AAA for all body sizes ✓
  - `--ink-2 #4A4A46`: ~8.4:1 — passes AAA ✓
  - `--ink-3 #7A7972`: ~4.4:1 — passes AA for normal text, **fails AAA**, OK for large text. Used heavily for mono labels, .lbl-sm, .micro, callouts. **[unverified-visual.]** At 9–10.5px (.lbl-sm L218, .micro L104) the labels are technically below normal-text body size — WCAG considers anything <18px (or <14px bold) as "normal text" requiring 4.5:1, and ink-3 sits at ~4.4:1 which is *just below*. This is borderline. Recommend tightening `--ink-3` from `#7A7972` to `#6F6E68` or similar to clear 4.5:1 cleanly.
  - `--accent #3D5A6C`: ~7.1:1 — passes AAA ✓
- **Dashed callouts** (`.drw-dash` stroke `--ink-3`, L214) — by design low-contrast, used for non-essential information (dimension lines, callout leaders). Acceptable visually, but the text labels at the end of those leaders should be at least `--ink-2`. Spot-checking L800 ("A · NOTCH · TRUEDEPTH" uses `.lbl-sm` → fill `var(--ink-3)`) — so callout label text inherits the borderline contrast. **[unverified-visual flag.]**
- **Focus rings:** see What's weak #2 — the only interactive element strips outline and only changes color/border. Real accessibility defect. The "blueprint look" can be preserved with a 1px dotted accent outline or `outline: 1px solid var(--accent); outline-offset: 2px;` — both fit the doc voice.
- **Reduced-motion:** end-to-end correct. §00 reduced-motion path (L357–378) was designed in, not bolted on. Best example of accessibility-as-design-input on the page.
- **ARIA:** `aria-labelledby` used consistently on every section + SVG `<title>` on every SVG (L637, L735, L839, etc.). Replay button has explicit `aria-label`. Crumb has `aria-label="document path"`. **Strong.**
- **Semantic HTML:** `<main>`, `<section>`, `<article>` on elevation cells, `<table>` with `<thead>`/`<th scope="col">` for the matrix, `<footer>`. Proper.
- **Keyboard navigation:** with only one focusable element (the replay button), tab-order is trivial — but that one tab stop must be visible. See focus-ring fix.
- **Animation safety:** no parallax, no auto-loop, no flash. Animation is opt-in via scroll-into-view + opt-replay. Safe.

---

## Brand / taste findings

- **Coherent published artifact, not hobby file.** The title-block / footer / doc-control framing carries the same voice from L600 to L1626. Every section uses the same `sec-hd` strip. Every SVG uses the same `.drw / .drw-soft / .drw-dash / .drw-fill / .drw-acc` stroke vocabulary. The blueprint metaphor isn't decoration — it's the spine.
- **Restraint is the point, and it holds.** One accent color, two paper tones, three ink steps, two fonts. The aesthetic constraint is doing the heavy lifting, exactly as the README claims (L76 in README: "one accent color forces visual hierarchy through weight and size, not hue"). The page lives by that.
- **Every element earns its space — almost.**
  - **Earning their space:** the scale-bar (L607–615), the corner registration marks (L572–579), the §00 re-plot animation, the §02 Δ column, the §04 migration channel concept, the §05 lifespan band in accent-tint, the title-block side panel.
  - **Borderline:** the §02 "Weight +16 g · +8%" row — small delta, present mostly to be exhaustive. Defensible under doc voice.
  - **Not earning:** I couldn't find an element that's pure decoration. Even the faint background grid is doing structural work.
- **Inconsistencies I found:**
  - Footer values are sans 12px (L568); title-block values are mono 13px (L127). Minor — see Footer findings.
  - `.replot-replay` strips outline (L273) — only place a focus style is overridden. Inconsistent with the otherwise-careful accessibility posture.
  - README references colors and components that don't match the build (paper hex, Exynos model name). Not a render issue but a "single source of truth" defect.
- **Voice:** the prose is the right register — terse, doc-style ("subsystem by subsystem", "Δ delta", "primary device", "release · personal record", "Specs per OEM datasheets. No warranty implied."). Not corporate, not whimsical. The whole page committed to "engineering document" as a voice and didn't blink.

---

## Prioritized improvement list

### MUST

1. **Fix §04 responsive collapse.** Add `min-width: 1000px` + horizontal scroll wrapper around the architecture SVG (mirror the §05 pattern at L546–548). Without this the section is unusable below ~900px and that's where most users live.
2. **Restore a visible focus indicator on `.replot-replay`** (L273). Replace `outline: none` with `outline: 1px solid var(--accent); outline-offset: 2px;` on `:focus-visible`. Keep the hover color/border-color swap as-is.

### SHOULD

3. **Grow §00 replay button touch target.** Bump padding from `5px 10px` to `10px 14px` and add `min-height: 32px` / mobile `min-height: 36px`. Currently below WCAG 2.5.5 AAA threshold; borderline AA.
4. **Amplify §00 stagger** from 0–0.45s to 0–1s across tiers, OR compress the per-stroke draw window so the existing stagger reads as deliberate beats. Without one of these the "outline → details" choreography is invisible.
5. **Trim Hero `.subt`** (L605) by half. The doc voice rewards terseness; 105 words is prose, not annotation. Target ≤50 words.
6. **Tighten `--ink-3`** from `#7A7972` to ~`#6F6E68` to clear 4.5:1 contrast on small mono labels cleanly. Borderline currently.
7. **Reconcile README ↔ CSS** on paper color hex and Exynos model number. README is stale. Update README to match the build.

### NICE

8. **Add accent-tint zebra-striping** to the §02 matrix tbody only if a render confirms it reads monotone (`tr:nth-child(even) { background: var(--paper-2) }`). Don't add prophylactically.
9. **Increase elevation SVG max-width** from 260px to ~320px (L193) so the silhouettes breathe more at desktop widths.
10. **Unify footer value typography** to mono 12px to match title-block side-panel values. Minor coherence polish.
11. **Demote one of the three revision strings** (crumb at L599 vs title-block L619 vs footer L1608). Pick one canonical, drop or soften the others.
12. **Consider a single-line "as-built figures sourced from manufacturer datasheets" note** appearing once at the top (under §00 or in the title block) rather than scattered in §02 footer (L1222) and footer notes (L1623). Document-style consistency.

---

## Unresolved questions

- **§00 cycle length [unverified-visual]:** does 11.5s feel right on first view, or is the "hold drawn" window too long? Recommend a render-test before tweaking.
- **§01 dimensional callouts at mobile [unverified-visual]:** `.lbl-sm` 8px scales with SVG — does it stay legible at 360px width? Source math says ~5.8px effective. May need a mobile-only viewBox tweak or larger label class.
- **§02 matrix monotony [unverified-visual]:** 14 rows of identical paper background — does it read clean (current intent) or visually flat? Decides whether suggestion #8 is needed.
- **§04 paired-row accent connectors** (L1452–1453, etc.): two 1px accent strokes per row between stack and channel — at desktop do they read as connectors or as visual noise? **[unverified-visual.]**
- **§05 lifespan band color:** `--accent-tint #EEF1F3` filling the 14px-tall band — does it read as a distinct "device era" zone, or is it so close to paper-2 that it blends? **[unverified-visual.]**
- Is README intended to be authoritative or is the HTML the single source of truth? Determines which side of suggestion #7 to update.

---

**Status:** DONE_WITH_CONCERNS
**Summary:** Strong artifact — token discipline, voice, and centerpiece animation are all earned. One real responsive defect (§04 below ~900px) and one accessibility defect (focus ring stripped on replay button) are MUST-fix. Several SHOULD/NICE items follow. Five claims tagged [unverified-visual] need a live browser render to confirm.
