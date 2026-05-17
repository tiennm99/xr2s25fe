# Brainstorm — Apple-style redesign for xr2s25fe

**Mode:** Advisory. No source files modified.
**Read:** `index.html` (sections / tokens / SVG geometry); `plans/reports/review-260517-1513-design-quality.md`; `README.md`.
**Date:** 2026-05-17

---

## TL;DR

**Recommend Concept C — "Keynote Spec Sheet"** (Apple product-page archetype + a hint of compare-page): full-bleed dark hero, one-idea-per-screen scroll, oversized numerals as the centerpiece, SVG silhouettes promoted to "hero stage" treatment, §02 matrix kept as a compact Apple-compare table. Live alongside as `apple.html`. Lean into the iPhone-leaving-via-Apple-aesthetic irony only at the close (footer + Easter-egg crumb). It reuses the most existing assets, hits hardest visually, and keeps the static one-file constraint.

---

## Design space framing

"Apple style" is ~6 distinct house styles. The viable poles for this content: **product page** (apple.com/iphone-16-pro/ — long-scroll, full-bleed sections, big type, dark/light alternation); **compare page** (apple.com/shop/buy-iphone/compare — clean table, sticky headers, tight rows); **newsroom** (apple.com/newsroom/ — single-column editorial, credibility voice); **developer** (developer.apple.com/design/ — technical but polished, closer to current blueprint). Keynote / event pages are a *mode* of the product page, not a separate archetype. Support is too plain for this purpose. Most concepts below hybridize product + one other.

---

## Concept A — "Spec Sheet, Cupertino Edition"

1. **Name.** Spec Sheet, Cupertino Edition.
2. **Archetype.** **Apple developer / docs** (developer.apple.com/design/human-interface-guidelines/) crossed with **compare page**. Minimal, technical, restrained.
3. **Pitch.** Keep the current page's data-density bones; reskin to Apple's docs/HIG aesthetic — SF Pro, generous whitespace, soft sectional dividers, no theatrics.
4. **Aesthetic fingerprint.** Light theme only. `#FBFBFD` (Apple's near-white) on `#1D1D1F` (Apple's near-black). SF Pro Text 17px body, SF Pro Display for headings, SF Mono for numerals. 8pt grid. No background grid pattern, no registration marks, no scale-bar. Hairline `#D2D2D7` dividers between sections.
5. **Hero.** Single centered headline ("Seven years. One device.") at SF Pro Display 56px semibold; one-line subhead; the §00 plot animation moves *below the fold* as the first scrolled section.
6. **Section translation.**
   - Hero → centered single-column, no title block, no crumb.
   - §00 → kept, but plotter stage becomes a `#F5F5F7` rounded card with 16px corner radius. Tier stagger amplified per prior review.
   - §01 → 2×2 grid kept; cell chrome stripped to a single `#D2D2D7` divider; silhouette strokes thickened to 1.5px.
   - §02 → restyled as Apple compare-page: sticky `<thead>`, alternating row tint `#F5F5F7`, model thumbnails at top of each column.
   - §03 → kept; bars become rounded (radius 4) with a soft `#0071E3`-tint fill (Apple developer blue, not steel-blue).
   - §04 → kept; SVG re-rendered in two-color (ink + Apple-blue tint), `min-width: 1000px` + `overflow-x: auto` fix from prior review carried over.
   - §05 → kept; lifespan band swaps to `#0071E3` at 10% opacity.
   - Footer → collapsed to two lines, centered. Drop "DEV-EVO-001" / "Sheet 1 of 1".
7. **Animation.** Subtle. `prefers-reduced-motion`-respecting fade-in on section enter (IntersectionObserver, 400ms, ease-out, no Y-translate). §00 plotter kept.
8. **Voice.** Hybridize. Current dry-engineering register is closer to Apple's *developer* register than its marketing voice. Keep facts; soften imperatives.
9. **Color.** Single light theme. Accent shifts steel-blue → Apple developer blue `#0071E3`. Two greys (`#1D1D1F`, `#86868B`). That's it.
10. **Preserved.** All SVGs, all spec data, §00 animation, the data-density discipline.
11. **Removed.** Background grid; corner registration marks; scale-bar; title-block side-panel; crumb; "Rev A — 2026.05" framing language; mono-everything for labels.
12. **Tech.** Self-contained `index.html`. SF Pro served from `https://fonts.cdn-apple.com/` with system-ui fallback. IntersectionObserver for fade-ins. No build.
13. **Pros / Cons / Risks.**
    - **Pros.** Lowest risk; highest reuse of existing structure; respects existing data density; familiar to anyone who's read developer.apple.com.
    - **Cons.** Doesn't feel "Apple consumer" — feels Apple-internal. Loses the document-as-artifact identity that makes the current page feel *intentional*. Reads as "current page with different fonts".
    - **Risks.** Lands as a downgrade in personality vs. current blueprint version. Apple developer aesthetic is restrained but not memorable.
14. **Complexity.** **Low.** ~+150 / -300 LoC. Net file shrinks.

---

## Concept B — "Newsroom Long-Read"

1. **Name.** Newsroom Long-Read.
2. **Archetype.** **Apple newsroom** (apple.com/newsroom/) + a hint of editorial print.
3. **Pitch.** Treat the upgrade as a press release: single-column, generous line-length, restrained, sober, credibility-first. The spec table appears mid-article like a sidebar.
4. **Aesthetic fingerprint.** Pure white `#FFFFFF` on `#1D1D1F`. SF Pro Display for headlines (lower weights — 400/500 only), SF Pro Text 19px body at 1.7 line-height. Max content width 680px (newsroom paragraph width). No grid. No accent color in body; one accent `#0066CC` for links only.
5. **Hero.** Newsroom-style: small uppercase kicker ("PERSONAL · UPDATE · 2026") → long-form headline ("After seven years, the device under my fingers changed.") → byline → 1-line dek.
6. **Section translation.**
   - Hero → newsroom headline block.
   - §00 → moved to *figure 1* under the lede; caption-style label below the SVG.
   - §01 → silhouettes presented as paired "figure 2 / 3", each with newsroom-style caption text underneath. 2×2 collapses to single-column at all widths.
   - §02 → demoted to a **collapsible** technical appendix at the end (Apple newsroom pattern: long article, specs at bottom).
   - §03 → reframed as inline bar-charts beside paragraphs, smaller (max-width 480px).
   - §04 → kept but redrawn as a horizontal flow chart, photo-caption styling.
   - §05 → simplified to a 1-line timeline with three milestone callouts above body text.
   - Footer → newsroom-style: "Press contact" → "Author"; ISO date.
7. **Animation.** None. Newsroom = no animation. Plotter becomes a static "before/after" two-up SVG.
8. **Voice.** Adopt Apple-newsroom voice. Third person, declarative, no hype. "The XR shipped in October 2018." Not "The XR was a legendary phone."
9. **Color.** Pure white only. No alternation. No tint. Single link blue.
10. **Preserved.** All spec data, all SVG geometry (restyled). Article structure replaces document structure.
11. **Removed.** Plotter animation (becomes static). Most chrome — title block, scale-bar, crumb, registration marks, background grid. Δ accent column simplified to plain text.
12. **Tech.** Self-contained `index.html`. No JS. Single CSS block. Easiest concept to build.
13. **Pros / Cons / Risks.**
    - **Pros.** Most "earned" Apple-fidelity (newsroom is one of Apple's calmest, most signature pages and is achievable without product photography). Plays the irony straight — Apple-style press release announcing the user left Apple. Strong tonal commitment.
    - **Cons.** Kills the §00 animation, which the prior review called the centerpiece. Loses visual density that justifies the page existing. Risk of feeling like a blog post.
    - **Risks.** Without imagery, newsroom layout can feel sparse. Page becomes a *piece of writing* — the user may not want a written piece.
14. **Complexity.** **Low.** Big subtractive rewrite. ~-700 LoC net.

---

## Concept C — "Keynote Spec Sheet" ⭐ RECOMMENDED

1. **Name.** Keynote Spec Sheet.
2. **Archetype.** **Apple product page** (apple.com/iphone-16-pro/) with **compare page** for §02 only. Hybrid.
3. **Pitch.** Full-bleed dark hero with massive headline; long-scroll one-idea-per-screen pacing; SVG silhouettes promoted to "stage" treatment with big numerals overlay; §00 plotter survives as the second screen; compare-style table for the matrix. Stops short of being a fake Apple ad — keeps doc voice in the body copy.
4. **Aesthetic fingerprint.**
   - **Alternation:** dark sections (`#000000` paper, `#F5F5F7` ink) for Hero / §00 / §03 / §05; light sections (`#FBFBFD` paper, `#1D1D1F` ink) for §01 / §02 / §04 / Footer.
   - **Type:** SF Pro Display — `clamp(40px, 7vw, 96px)` for section headlines, 600 weight, letter-spacing -0.04em (Apple's signature display setting). SF Pro Text 19px for body. SF Mono 13px for spec numerals.
   - **Spacing:** generous. Each section min-height ~80vh. 8pt grid.
   - **Signature elements:** full-bleed sections, scroll-snap on `<section>` (with `scroll-snap-type: y proximity`), oversized numerals as visual anchors (e.g. "**+5GB**" at 200px), no hairline borders between sections (color flip is the divider).
5. **Hero.** Black background. Centered. Two-line headline: "**Seven years. One upgrade.**" / "**Documented, dimension by dimension.**" SF Pro Display 96px (clamped). Subhead under: 19px body, 60ch max. Scroll-hint chevron at the bottom (`prefers-reduced-motion`: static).
6. **Section translation.**
   - **Hero** → dark, centered, oversized headline. Title-block / scale-bar / crumb **gone** — but the four metadata pairs (Document / Revision / Span / Sheet) reappear in the footer as a quiet Easter-egg homage to the blueprint version.
   - **§00 Transformation** → KEEPS the plotter. Dark background. SVG silhouettes now stroked in `#F5F5F7` on black. Stage becomes full-bleed. Stagger amplified (per prior review's MUST). Headline overlay "**Plot. Erase. Re-plot.**" appears in big SF Pro behind/above the SVG (z-stacked, low contrast).
   - **§01 Elevations** → Light. 2×2 grid kept (mobile collapses to 1-col). Each cell is a `#F5F5F7` rounded card (radius 18, Apple product-card radius). Silhouettes scaled up to ~360px (prior review: cap was too small at 260). Dimension callouts kept but smaller / lower contrast.
   - **§02 Spec Matrix** → Light. Switches to **Apple compare-page table**: sticky `<thead>`, small model thumbnails at the top of each column (reuse two §01 silhouettes at 80px), hairline `#D2D2D7` row dividers, Δ column preserved but renamed "Change" with `#0066CC` tint. Mobile collapse pattern preserved.
   - **§03 Capability Deltas** → DARK. Each bar pair gets one screen. 2×4 grid → vertical stack of 8 screens with scroll-snap (or 2×4 grid in compact mode — see open question). Numerals rendered HUGE ("**3 GB → 8 GB**" at 120px). Bars are thinner, accent `#0066CC`.
   - **§04 Architecture** → Light. SVG redrawn at higher contrast. Apply prior review's MUST fix: `min-width: 1000px` + `overflow-x: auto` wrapper. Keep migration channel concept; restyle legend.
   - **§05 Timeline** → DARK. Horizontal scroll preserved. Lifespan band recolored as `#0066CC` gradient (10% → 30% opacity along the band). Milestones get larger dots; XR launch and S25 FE launch get callout cards.
   - **Footer** → Light. Single line of body copy + author + ISO date + the Easter-egg 4-cell metadata grid as small print.
7. **Animation.** Section enter: 400ms fade + 12px Y-translate, ease-out, IntersectionObserver, once. §00 plotter preserved with current 11.5s cycle (stagger amplified). Scroll-snap **proximity** only (not mandatory) so trackpad/wheel users aren't fought. Oversized numerals on §03 get a subtle count-up animation when in view. `prefers-reduced-motion: reduce` removes all of the above.
8. **Voice.** Hybridize, lean *Apple keynote* in section headlines and *current doc voice* in body. Headlines bold and short ("Seven years.", "From 3 to 8.", "One charger to rule them."). Body text stays terse and factual. Avoids the falseness of full marketing voice while earning the Apple format.
9. **Color.** Two-tone alternation (#000 / #FBFBFD). One accent: `#0066CC` (Apple system blue) for callouts, deltas, lifespan band. No second accent.
10. **Preserved.** All SVG geometry (recolored). All spec data. §00 plotter (the centerpiece). The 4-cell title-block metadata (as footer Easter egg). The Δ column. The architecture migration channel. The timeline.
11. **Removed.** Background grid; corner registration marks; scale-bar; "Rev A — 2026.05" crumb; "DEV-EVO-001" framing in the body; blueprint paper color; doc-control footer block; mono-for-all-labels.
12. **Tech.** Self-contained `index.html`. SF Pro from `https://fonts.cdn-apple.com/fonts/SF-Pro-Display.woff2` with `system-ui, -apple-system` fallback (SF Pro is already system font on Apple devices). IntersectionObserver for fade-ins + count-up. CSS scroll-snap on `<html>`. No framework. Likely a small `<script>` for count-up only.
13. **Pros / Cons / Risks.**
    - **Pros.** Maximum visual impact. Reuses all existing assets (SVGs, spec data, plotter). Hits the Apple product-page archetype which is *the* most recognizable Apple style. Big-numeral §03 plays directly into the page's "+5 GB · 2.67×" Δ voice — the data was already designed for this format. Earns the Apple comparison without faking product photography (the silhouettes substitute).
    - **Cons.** Biggest delta from current page → biggest reviewer-fatigue tax (rebuilds nearly every section's styling). Dark-mode introduces a second token tree → bigger CSS surface. The "fake Apple keynote" angle could read as parody if the body copy slips into Apple-marketing voice anywhere. Discipline required.
    - **Risks.** Without product photography, oversized type sections can feel hollow on widescreen. Mitigation: SVG silhouettes scale up to fill the void. Scroll-snap can fight users on wheel/trackpad — *proximity* mode mitigates but doesn't eliminate.
14. **Complexity.** **Medium-High.** ~+400 / -400 LoC. File grows modestly. New: dark-mode token tree, scroll-snap rules, oversized type clamp scale, IntersectionObserver script (~30 LoC).

---

## Concept D — "Compare-Page Purist"

1. **Name.** Compare-Page Purist.
2. **Archetype.** **Apple compare page** (apple.com/shop/buy-iphone/compare) — only.
3. **Pitch.** Drop everything except the comparison. The entire page is one giant sticky-header compare table. Silhouettes are column headers; spec rows are the body; the architecture / timeline / deltas become extra row-groups inside the same table.
4. **Aesthetic fingerprint.** Light theme only. `#FBFBFD` background. SF Pro Text 17px, SF Pro Display 24px for row-group headings. Hairline `#D2D2D7` dividers. Centered, max-width 980px (Apple compare is narrow). Each device gets a column; silhouettes pinned at top of each column.
5. **Hero.** Tiny. Single line: "Compare." (Apple's literal compare-page hero is just the word.) Below: two device names + silhouettes side-by-side.
6. **Section translation.**
   - Hero → reduced to one word.
   - §00 → **removed** (compare pages don't animate).
   - §01 → silhouettes promoted to column headers; remaining elevation details dropped.
   - §02 → THIS IS THE PAGE. Expanded to absorb everything else.
   - §03 → folded into the matrix as bars within table cells.
   - §04 → folded in as row-groups (OS / SoC / MEM / AUTH / I/O / RF / DIST).
   - §05 → 1-line timeline at the bottom of the matrix as a "ownership span" row.
   - Footer → compare-page style, minimal disclaimers.
7. **Animation.** None.
8. **Voice.** Apple compare voice — labels only, no prose. Every spec is a noun phrase. "Display." / "Battery life." / "Camera system."
9. **Color.** Light. One accent: `#0066CC` for Δ column.
10. **Preserved.** All spec data; silhouettes (cropped to column-header size); table.
11. **Removed.** Hero language, plotter animation (centerpiece), architecture diagram (folded into rows), timeline as a visual (folded to one row), most prose.
12. **Tech.** Self-contained `index.html`. Tiny. ~250 LoC total possible.
13. **Pros / Cons / Risks.**
    - **Pros.** Most disciplined Apple-fidelity to a specific page. Tightest, fastest, smallest. Anyone who's used apple.com/compare instantly recognizes the format.
    - **Cons.** Kills the §00 plotter (centerpiece per prior review). Kills the §04 architecture diagram (strongest single image at desktop per prior review). The "long upgrade story" becomes a flat lookup table. Loses the *personal* angle entirely.
    - **Risks.** Page becomes data without narrative. May feel like a spec aggregator rather than an upgrade record.
14. **Complexity.** **Low.** ~-900 LoC net. Smallest output of any concept.

---

## Concept E — "Two-Tone Diptych"

1. **Name.** Two-Tone Diptych.
2. **Archetype.** **Apple product page** structure + an old Apple "iPod vs. Zune" / "Mac vs. PC" visual rhetoric. Two columns. One for XR, one for S25 FE. Side-by-side full-page diptych.
3. **Pitch.** Vertical split — left half "2018 / iPhone XR" against right half "2025 / S25 FE" — running the full page height. Both columns scroll together. Each section is a paired horizontal slice.
4. **Aesthetic fingerprint.** Light-on-left (`#FBFBFD`, paper-like, retro-feel), dark-on-right (`#1D1D1F`, modern). SF Pro Display headings. Vertical 1px hairline divider down the center.
5. **Hero.** Top-of-page diptych: "2018 / XR" left, "2025 / S25 FE" right. Below: silhouettes scaled large in each half. No central headline at all — the split *is* the headline.
6. **Section translation.**
   - Hero → diptych top.
   - §00 plotter → **moved center** — plotter sits in the middle gutter as a transition between hero and §01.
   - §01 → each elevation pair splits across the diptych: XR front + XR rear on left, S25 front + S25 rear on right. Two rows.
   - §02 → matrix collapses to two columns (XR / S25 FE); Δ becomes a centered chip between them at each row. Sticky thead.
   - §03 → bar pairs split across the diptych: XR bar grows leftward from center, S25 FE bar grows rightward. Beautiful but novel.
   - §04 → architecture diagram is already two-column (Unit A / Unit B with migration channel center). NATIVE FIT for this concept. The migration channel IS the page's center gutter.
   - §05 → timeline straddles the gutter horizontally at the bottom; XR launch on left, S25 FE launch on right.
   - Footer → spans the full width, dark.
7. **Animation.** Scroll-driven center-gutter parallax: §00 plotter slides through gutter as page scrolls. §04 connector lines animate left-to-right on scroll-into-view (architecture migration). `prefers-reduced-motion: reduce` removes both.
8. **Voice.** Comparative, taunting in a friendly way. "3 GB." / "8 GB." in each column. Body copy minimal.
9. **Color.** Strict two-tone, light vs. dark. Accent `#0066CC` reserved for the migration channel / Δ chips only.
10. **Preserved.** All spec data; all SVGs (split across the diptych); §00 plotter; §04 migration concept *amplified*.
11. **Removed.** Most chrome. Background grid; title block; crumb; registration marks; scale-bar.
12. **Tech.** Self-contained `index.html`. CSS Grid for the diptych (2-col with 1px gutter). Mobile (≤720px): diptych collapses to alternating left/right slabs stacked vertically. IntersectionObserver for connector animations.
13. **Pros / Cons / Risks.**
    - **Pros.** §04 migration channel becomes the *entire page's architectural metaphor* — extremely strong conceptual unification. Visually most distinctive of any concept. The diptych split *is* the story.
    - **Cons.** Hardest to make work on mobile — diptych collapse is non-trivial. Risk of feeling gimmicky. §03 bars growing from center is novel territory — could read as confusing.
    - **Risks.** Responsive collapse adds significant complexity. Mobile layout has to abandon the diptych entirely, making the desktop and mobile experiences feel like different sites.
14. **Complexity.** **High.** ~+600 / -400 LoC. Most CSS work. Most responsive risk.

---

## Comparison table

| Concept | Taste / Apple-fidelity | Complexity | Fits existing content | A11y | Responsive risk | Irony management |
|---|---|---|---|---|---|---|
| A. Cupertino Edition | Med (developer pole — recognizable but not signature) | **Low** | High (minimal change to structure) | High | Low | Plays it straight — irony minimal |
| B. Newsroom Long-Read | **High** (newsroom is one of Apple's most signature reduced pages) | Low | Low (kills §00, demotes §02) | High | Low | **Direct** — Apple press release announcing user left Apple. Tonal commitment. |
| C. Keynote Spec Sheet ⭐ | **High** (product-page archetype — *the* canonical Apple style) | Med-High | **Very high** (reuses every asset, amplifies §00 + §03) | Med (dark mode, scroll-snap, type contrast — all manageable) | Med (scroll-snap proximity + standard breakpoints) | **Calibrated** — format is Apple, voice stays sober. Easter egg in footer. |
| D. Compare-Page Purist | High (literal Apple compare clone) | **Low** | Low (kills §00 + §04 + most narrative) | **High** (table semantics) | Low | Subtle — page format reads as Apple's, no overt joke |
| E. Two-Tone Diptych | High (novel synthesis, not a literal Apple page) | **High** | Med-high (§04 fits natively; §03 risky) | Med-low (split layout for screen readers needs care) | **High** (mobile collapse) | Strong — visual rhetoric of "Apple left, Samsung right" |

---

## Recommendation

**Concept C — Keynote Spec Sheet.** Defense:

1. **Asset reuse is highest.** SVG silhouettes, §00 plotter, spec data, Δ column, architecture migration channel, timeline — all survive and most are *amplified*. Concepts B and D throw away the §00 plotter, which the prior design review identified as "the kind of 'earned' centerpiece that justifies the page existing".
2. **Apple-page-archetype match is canonical.** Long-scroll product page is *the* recognizable Apple style. Newsroom (B) is recognizable to Apple-readers but few others; developer (A) is recognizable only to engineers; compare-purist (D) clones a single utility page.
3. **Big numerals are pre-designed for this format.** §03 already says "× 2.67", "+1958 mAh", "× 4.17", "4G → 5G" — those are *already* Apple-keynote numerals. The format change is mostly enlargement + alternation, not invention.
4. **Irony is calibrated, not weaponized.** Format = Apple. Voice stays factual. Footer Easter-egg metadata grid is the only wink. Avoids the parody risk of B's "press release leaving Apple" angle.
5. **Builds on existing fixes.** The prior review's MUST items (§04 responsive, focus-ring) carry into C cleanly. Concept D would orphan the §04 fix entirely.
6. **Runner-up: Concept E** (Diptych). More conceptually unified but mobile risk is too high for a one-file constraint. Reconsider only if user prefers max distinctiveness over responsive simplicity.
7. **Co-existence:** ship as **`apple.html`**, keep current `index.html` as the blueprint version. Two pages, two voices, one repo. Link in each footer ("see the blueprint version" / "see the Apple version"). Cross-linking the two *is* the meta-joke without compromising either page.

---

## Phased rollout for Concept C

**MVP (1 day-ish):**
1. Fork `index.html` → `apple.html`.
2. Replace token tree: paper / ink → `#000` / `#FBFBFD` for dark sections, `#FBFBFD` / `#1D1D1F` for light. Single accent `#0066CC`. Drop the grid background and registration marks.
3. Swap fonts: Inter → SF Pro Display + SF Pro Text (cdn-apple.com, system-ui fallback). JetBrains Mono → SF Mono.
4. Hero: full-bleed dark, oversized centered headline, no title-block.
5. §00, §01, §02, §03, §04, §05, Footer keep their HTML structure. Apply dark/light alternation via section class. Pull existing SVGs through unchanged except stroke color binding.
6. Carry forward MUSTs from prior review: §04 `min-width` + `overflow-x: auto`; focus-visible ring on replay button.

**Polish (0.5 day-ish):**
7. Amplify §00 plotter stagger (already a SHOULD from prior review).
8. Oversized numerals in §03 (clamp 80px → 160px) with `.lbl` repositioning.
9. IntersectionObserver fade-in script (~30 LoC).
10. `prefers-reduced-motion` audit: fade-ins off, scroll-snap off, count-up off, plotter falls back to static end state (already done in current site).
11. Footer Easter-egg metadata grid (4 cells: Document / Revision / Span / Sheet) restyled as small print.

**Stretch (skip if YAGNI complains):**
12. CSS scroll-snap proximity on sections.
13. Count-up animation on §03 numerals (only triggers once, only with motion preference allowed).
14. Sticky compact nav appearing after hero scroll-out (Apple product pages do this — small "Buy" button equivalent could be a "View blueprint version" cross-link).

---

## Open questions

- **§03 layout commitment:** keep current 2×4 grid (compact, easy responsive) or break each delta into its own scroll-snap screen (8 vertical screens, more dramatic, longer page)? Recommendation in C assumes the 2×4 grid stays with enlarged numerals; full-screen-per-delta is a stretch option.
- **SF Pro hosting:** load from cdn-apple.com (cleanest, but technically Apple-served fonts), bundle a self-hosted copy, or rely entirely on `system-ui`/`-apple-system` (renders SF Pro on Apple devices, Segoe / Roboto elsewhere)? User-choice — recommendation defaults to cdn-apple.com with system fallback.
- **Co-exist vs. replace:** ship `apple.html` alongside `index.html` (recommended, preserves blueprint version + sets up the meta-joke), or replace `index.html` entirely (simpler but destroys the blueprint version which the design review called "a coherent published artifact")?
- **Voice slippage:** how hard to commit to keynote-headline voice in section titles? "Seven years." vs. "Transformation · re-plot". Recommend Apple-style headlines + parenthetical mono subhead preserving original section names — gets both registers.
- **Cross-link discoverability:** if shipping both pages, how prominent should the link between them be? Recommendation: footer-only on both, no nav. Discovery is the reward.
- **Irony index:** how openly should the Apple-format-for-Apple-leaving-page joke surface? Currently the recommendation hides it (footer Easter egg + cross-link only). User may want it more or less surfaced.

---

**Status:** DONE
**Summary:** Five concepts (developer, newsroom, product-keynote, compare, diptych); recommend Concept C — "Keynote Spec Sheet" — long-scroll Apple product-page archetype, dark/light alternation, oversized numerals, all existing SVGs and §00 plotter preserved; ship as `apple.html` alongside current blueprint `index.html`.
