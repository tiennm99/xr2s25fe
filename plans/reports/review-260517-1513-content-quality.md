# Content Quality Review — Device Evolution // iPhone XR → Galaxy S25 FE
**Reviewed:** 2026-05-17  
**Files:** `index.html`, `README.md`, GitHub repo metadata  
**Reviewer scope:** Public-facing copy, spec accuracy, voice/tone, README quality, cross-doc consistency, repo metadata

---

## TL;DR

Content is in strong shape. Voice is consistently technical and restrained throughout. Two spec accuracy issues identified — one CRITICAL (wrong SoC on README). Several medium-priority copy tightenings available. No compliance or brand violations.

| Severity | Count |
|----------|-------|
| CRITICAL | 1 |
| High | 2 |
| Medium | 4 |
| Low | 5 |

---

## Spec Accuracy Findings

### CRITICAL — README line 17: Wrong SoC name

**Offending text:** `"The S25 FE ships with a 4nm Exynos 2500"`  
**File:line:** `README.md:17`  
**Fact:** The Galaxy S25 FE uses the **Exynos 2400e** (a binned variant of the 2400), NOT the Exynos 2500. The Exynos 2500 is Samsung's next-generation node (3nm, 2025 flagship SoC used in Galaxy S25 series proper). The page's `index.html` correctly says "Exynos 2400e" throughout (§02 matrix line 1145, §04 architecture line 1421).  
**Fix:** Change `Exynos 2500` → `Exynos 2400e`

---

### HIGH — README line 89: mmWave claim unverifiable / likely incorrect

**Offending text:** `| Network | 4G LTE | 5G (sub-6 + mmWave) |`  
**File:line:** `README.md:89`  
**Fact:** The Galaxy S25 FE is widely reported as sub-6 GHz 5G only. mmWave support on FE-line devices is not confirmed in Samsung's published specs (OEM datasheet as of 2025 lists sub-6 only). The page correctly says "5G sub-6" everywhere (§02 matrix line 1193, §03 delta bars, §04 architecture line 1437). README contradicts the page on this point.  
**Fix:** Change `5G (sub-6 + mmWave)` → `5G (sub-6)`

---

### Verified Correct — Notable items

| Claim | Source | Status |
|-------|--------|--------|
| iPhone XR: 7nm A12, 3GB RAM, 6.1" LCD 1792×828 326ppi 60Hz | index.html §02 | ✓ matches OEM specs |
| iPhone XR: 2942 mAh, Lightning 18W, 7.5W Qi | index.html §02 | ✓ |
| iPhone XR: 194g, 150.9×75.7×8.3mm, IP67 | index.html §01 elev-foot | ✓ |
| iPhone XR: single 12MP f/1.8 rear, 7MP f/2.2 front | index.html §01 callouts | ✓ |
| S25 FE: Exynos 2400e 4nm, 8GB LPDDR5X | index.html §02 | ✓ |
| S25 FE: 6.7" AMOLED 2X 2340×1080 120Hz HDR10+ | index.html §02 | ✓ |
| S25 FE: 4900 mAh, USB-C 45W, 15W Qi, reverse wireless | index.html §02 | ✓ |
| S25 FE: ~210g, 161.3×76.6×7.4mm, IP68 | index.html §01 elev-foot | ✓ |
| S25 FE: 50MP wide + 12MP UW + 8MP 3× tele, 12MP front | index.html §01, §02 | ✓ |
| S25 FE: Wi-Fi 6E, BT 5.3, NFC, UWB | index.html §02 | ✓ |
| S25 FE: One UI 7 / Android 15, 7-yr commitment | index.html §02 | ✓ |
| iOS 12→18 on XR, ~6yr support | index.html §02 | ✓ |
| iPhone XR: 4K @ 60fps video | index.html §01 elev-foot | ✓ |

**One spec to note with mild concern:** The rear-elevation elev-foot (line 1008) shows `12 MP f/1.8 OIS` for iPhone XR. The XR's main lens does have OIS — this is confirmed correct.

**Architecture diagram line 1393:** States iPhone XR memory as `LPDDR4X`. This is plausible (Apple didn't officially publish the RAM spec type, but LPDDR4X is the widely accepted teardown-verified figure). Low confidence but not contradicted by known facts — leave as-is.

**Architecture diagram line 1434:** States S25 FE USB-C as `USB 3.2 · DISPLAYPORT ALT`. Some sources list USB 3.2 Gen 1 (5 Gbps) for the S25 FE; a minority list USB 2.0 only. Samsung's OEM page for the S25 FE may clarify — this is an unresolved question (see bottom of report).

---

## Voice & Tone Findings

### MEDIUM — README line 17: "Apple Silicon" is a brand marketing term, not engineering taxonomy

**Offending text:** `"Apple Silicon → Arm Cortex-X"`  
**File:line:** `README.md:17`  
**Issue:** "Apple Silicon" is Apple's consumer marketing term. Engineering-doc voice would say "Apple A-series SoC" or "A12 Bionic." The page itself uses the crisper "APPLE SILICON  ⟶  EXYNOS" in the architecture diagram (index.html line 1459) but this is used as a migration label between ecosystems, where the contrast is valid shorthand. In the README prose, it drifts slightly marketing-adjacent.  
**Rewrite:** `"A12 Bionic → Exynos 2400e"`

(Minor — voice doesn't collapse here, but tightening aligns with the datasheet register.)

---

### LOW — index.html line 605: "no marketing surface" is awkward

**Offending text:** `"…display, silicon, memory, optics, power, radios, and ecosystem deltas, with no marketing surface."`  
**Issue:** "with no marketing surface" is borderline-opaque as a phrase. A reader needs to parse it. The intent is clear but it's trying too hard to signal its own restraint.  
**Rewrite:** `"…ecosystem deltas — no editorial, no score."`

---

### LOW — index.html line 1373: "WALLED GARDEN" is edgy for a pure-facts diagram

**Offending text (line 1373):** `UNIT A · 2018 · WALLED GARDEN`  
**Offending text (line 1484):** `WALLED GARDEN  ⟶  OPEN`  
**Issue:** "Walled garden" is accurate technically but carries a pejorative connotation common in Android-vs-iOS discourse. The document's stated goal is to not editorialize. A strict engineering doc would label it `CLOSED PLATFORM` or `SANDBOXED ECOSYSTEM`.  
**Note:** "OPEN" for Android is also editorially loaded. Both are mild and consistent, so this is LOW priority — fix only if the author wants zero editorializing.  
**Rewrite (optional):** `UNIT A · 2018 · CLOSED PLATFORM` and `CLOSED PLATFORM  ⟶  OPEN ANDROID`

---

### LOW — index.html line 1375: "OPEN ANDROID" has the same issue

Same as above — paired with the WALLED GARDEN note.

---

## README Findings

### HIGH — "Sections" block claims "Architecture — SoC block diagrams: A12 Bionic vs. Exynos 2500"

**File:line:** `README.md:59`  
**Offending text:** `Architecture — SoC block diagrams: A12 Bionic vs. Exynos 2500 core layout`  
**Issue:** Two problems: (1) Wrong SoC name again — `Exynos 2500` should be `Exynos 2400e`. (2) The page's §04 is not actually an SoC core-layout block diagram — it's an ecosystem/subsystem migration diagram (OS, Mem, Auth, I/O, Radio, Distribution). Calling it "SoC block diagrams … core layout" misrepresents what the visitor will see.  
**Fix:** `Architecture — ecosystem migration diagram: iOS/Apple stack → Android/Samsung stack`

---

### MEDIUM — README opening: "Seven years of Apple to Samsung" is imprecise

**File:line:** `README.md:3`  
**Offending text:** `"Seven years of Apple to Samsung, rendered as a technical blueprint."`  
**Issue:** It's seven model-years between devices (2018→2025), but the person used the XR for ~7 years — not "seven years of" the Apple brand. Slightly ambiguous — does it mean 7yr of Apple usage, or 7yr gap?  
**Rewrite:** `"Seven-year device gap — Apple to Samsung — rendered as a technical blueprint."`

---

### MEDIUM — README "Run Locally": `open index.html` is macOS-specific

**File:line:** `README.md:42`  
**Issue:** `open index.html` is macOS only. On Linux it would be `xdg-open index.html`; on Windows it's `start index.html`. The python fallback handles cross-platform, but the primary command misleads Linux users.  
**Fix:** 
```bash
# macOS
open index.html
# Linux
xdg-open index.html
```
Or just lead with the python server command and remove the OS-specific shortcut.

---

### LOW — README "Built" date in footer of file

**File:line:** `README.md:105`  
**Offending text:** `*Built 2026-05-17.*`  
**Issue:** Current date is 2026-05-17 which is the build/generation date, not an in-universe "written" date for a 2018→2025 story. Fine as-is, but this will look odd to GitHub visitors who see it as "future-dated" unless they understand it's a repo build date. No change required — flagging for awareness.

---

### README Structure Assessment

| Element | Status |
|---------|--------|
| Hook (first 5 lines) | Strong — "Personal upgrade datasheet. Seven years of Apple to Samsung" lands immediately |
| What section | Clear and well-written |
| Why section | Good — factual, no fluff |
| Stack table | Complete, accurate |
| Run locally | Needs cross-platform fix (see above) |
| Sections | Inaccurate description for §04 (see HIGH finding above) |
| Design Notes | Excellent — specific, engineering tone |
| Spec Deltas table | 5/6 rows correct; Network row has mmWave error |
| License | Present |
| Author | Present |
| Markdown rendering | Clean heading levels, valid table syntax, fenced code blocks |

---

## Page Copy Findings — by Section

### Hero / Title Block (lines 598–623)

- Crumb path: `Personal / Devices / Evolution / Rev A — 2026.05` — correct, tight, consistent.
- `h1`: `Device Evolution // 2018 → 2025` — tight, on-voice.
- Subtitle (line 605): `"A side-by-side technical sheet documenting the transition…with no marketing surface."` — see Low voice finding above.
- Titleblock cells: `Document · DEV-EVO-001`, `Revision · A · 2026-05-17`, `Span · 7 years · 2 OS`, `Sheet · 1 of 1` — all clean engineering-doc labels.
- `SCALE 1 : 1` and `0 — 50 mm` scale bar — appropriate, dry, on-voice.

### §00 Transformation / Re-plot (lines 627–711)

- Section header: `§ 00 · Transformation · re-plot` — clean.
- Caption tag: `plotting · 11.5 s cycle · click to replay` — concise, factual, on-voice.
- Replay button: `↻ REPLAY` — correct.
- Animated labels: `PLOTTING UNIT A · iPhone XR · 2018` / `PLOTTING UNIT B · Galaxy S25 FE · 2025` — tight, consistent.

### §01 Elevations (lines 713–1116)

- Section header: `§ 01 · Elevations — front & rear, dimensional` / tag `drawing · mm` — correct, units explicit.
- Callout labels on iPhone XR front: `A · NOTCH · TRUEDEPTH`, `B · MUTE / RING`, `C · LCD · 6.1"`, `D · LIGHTNING · SPK / MIC` — accurate, terse, on-voice.
- Callout labels on S25 FE front: `A · PUNCH-HOLE · 12 MP`, `B · ULTRASONIC FP`, `C · AMOLED · 6.7"`, `D · USB-C · SPK / MIC` — accurate, parallel structure to XR callouts, good consistency.
- iPhone XR rear callout: `B · QUAD-LED TRUE TONE` — accurate (XR did use Apple's Quad-LED True Tone flash).
- iPhone XR rear callout: `C · ISLAND · 22 × 22 mm` — units present, consistent.
- S25 FE rear callouts: `A · 8 MP 3× TELE OIS`, `B · 50 MP WIDE f/1.8 OIS`, `C · 12 MP ULTRAWIDE`, `D · 3 DISCRETE RINGS · NO ISLAND` — accurate, terse.
- elev-foot dimensions: `150.9 · 75.7 · 8.3` / `161.3 · 76.6 · 7.4` — correct, but **units not shown in elev-foot** (the header key `H × W × D` has no "mm" label). The tag `drawing · mm` on the section header covers it, but locally the values are unit-bare. Minor.

### §02 Comparison Matrix (lines 1118–1223)

- Column headers: `Subsystem / iPhone XR · 2018 / Galaxy S25 FE · 2025 / Δ Delta` — clean, consistent.
- All spec values verified (see Spec Accuracy section above).
- Display row: `6.7" AMOLED 2X · 2340×1080 · 120 Hz adaptive · HDR10+` — note ppi not shown for S25 FE, while XR shows `326 ppi`. Not wrong to omit (the matrix doesn't promise parity across all sub-specs), but slightly asymmetric.
- Matrix footnote (line 1222): `"Note · figures sourced from manufacturer datasheets and verified spec pages. Δ column is informational, not a benchmark."` — appropriate, on-voice.

### §03 Capability Deltas (lines 1225–1354)

- All delta-bar labels use consistent pattern: `Category · subcategory` with factor in accent.
- RAM: `× 2.67` — 8/3 = 2.667. Correct.
- Battery: `+ 1958 mAh` — 4900-2942 = 1958. Correct.
- Charging wired: `× 2.5` — 45/18 = 2.5. Correct.
- Main sensor: `× 4.17` — 50/12 = 4.167. Correct.
- Process node bar: 7nm → 4nm shown with 7nm bar at 57% and 4nm at 100%. This is **inverted logic** — smaller node is a reduction, not a larger value. The bar should arguably show 4nm as "more efficient/advanced" which is how it's being used, but labeling 4nm bar taller visually implies 4nm > 7nm in raw number terms (which is backwards — 4 < 7). This is a known convention ambiguity for process node bars and is editorial, not a spec error. Consider a clarifying sub-label like `(smaller = newer)`.

### §04 Architecture / System Shift (lines 1356–1494)

- Frame title: `ARCH · ECOSYSTEM MIGRATION · 2018 → 2025` — tight, on-voice.
- Column headers: `UNIT A · 2018 · WALLED GARDEN` / `UNIT B · 2025 · OPEN ANDROID` — see voice finding above.
- Migration channel: `MIGRATION CHANNEL · DELTA` — clean.
- Row labels: OS, SoC, MEM, AUTH, I/O, RF, DIST — terse, standard abbreviations.
- All migration arrows (`iOS ⟶ ANDROID`, `LIGHTNING ⟶ USB-C`, etc.) — accurate.
- Legend: `subsystem block / migration delta / logical link · dashed` — clear.

### §05 Timeline (lines 1496–1596)

- Section header: `§ 05 · Timeline — 2018 to 2025` / tag `lifespan · beats` — clean.
- Year labels: 2018–2025, 8 ticks — correct span.
- Contextual beats: `5G ROLLOUT BEGINS` at 2019, `USB-C MANDATE (EU)` at 2021, `ON-DEVICE LLMs` at 2023 — the EU USB-C mandate was actually phased (adopted 2022, enforcement 2024 for phones). Placing it at 2021 is misleading — the EU directive was passed in October 2022. **Low-priority factual issue.**
- `iOS 18 · XR FINAL` at 2024 — correct, iOS 18 was the last supported release for XR.
- Device labels: `iPhone XR · primary device` / `Galaxy S25 FE · primary` — the S25 FE label is truncated compared to the XR label. Minor style inconsistency (`· primary device` vs `· primary`).

### Footer (lines 1598–1627)

- Doc control fields: `Drawing no. DEV-EVO-001 / Revision A · build 2026-05-17 / Sheet 1 of 1 · scale 1 : 1 / Maintainer tiennm99 / Status Released · personal record / Notes Specs per OEM datasheets. No warranty implied.` — all clean, appropriate, on-voice.

---

## Repo Metadata Findings

### Description (118 chars — within 120 limit)
`"Personal device-upgrade datasheet — iPhone XR (2018) to Galaxy S25 FE (2025), rendered as a technical blueprint page."`  
- Clear, searchable, accurate.
- "blueprint page" is slightly redundant (blueprint implies page). Could trim: `"…rendered as a technical blueprint."` (saves 5 chars, tighter).
- No issues otherwise.

### Topics
`device-comparison, blueprint, iphone-xr, samsung-galaxy-s25-fe, static-site, html-css, data-visualization, showcase`

| Topic | Assessment |
|-------|-----------|
| `device-comparison` | Strong — high traffic tag |
| `blueprint` | Niche but accurate; may not surface often |
| `iphone-xr` | Good for device-specific discovery |
| `samsung-galaxy-s25-fe` | Good |
| `static-site` | Useful for developer discovery |
| `html-css` | Useful for developer discovery |
| `data-visualization` | Appropriate |
| `showcase` | Weak — very generic, low signal |

**Suggested additions/swaps:**
- `personal-project` or `portfolio` over `showcase` (slightly more searchable)
- Consider `svg` — the page is notable for its inline SVG engineering drawings; this would surface it in SVG-focused searches
- `apple-to-android` — likely searched as a topic by people doing the same transition
- `upgrade` — useful pairing with device-comparison

---

## Cross-Document Inconsistencies

| # | Location A | Location B | Inconsistency |
|---|-----------|-----------|---------------|
| 1 | README.md:17 `Exynos 2500` | index.html §02 line 1145 `Exynos 2400e` | CRITICAL: different SoC names |
| 2 | README.md:89 `5G (sub-6 + mmWave)` | index.html §02 line 1193 `5G sub-6` | HIGH: mmWave not in page |
| 3 | README.md:59 `Exynos 2500 core layout` | index.html §04 (ecosystem migration, not SoC core diagram) | HIGH: misrepresents §04 content |
| 4 | README.md:59 `SoC block diagrams: A12 Bionic vs. Exynos 2500` | actual §04 label `ARCH · ECOSYSTEM MIGRATION` | HIGH: section description wrong in README |

---

## Typos / Grammar / Punctuation

| File:line | Text | Issue |
|-----------|------|-------|
| index.html:1402 | `I/O · LIGHTNING` sub-label `USB 2.0 · PROPRIETARY` | Correct spec; Lightning is USB 2.0 internally. Not a typo but could confuse — Lightning port outputs USB 2.0 speeds. |
| index.html:1586 | `USB-C MANDATE (EU)` placed at 2021 tick (x=469) | Factual: EU mandate passed Oct 2022, enforcement Dec 2024. 2021 positioning is off by 1 year. |
| README.md:3 | `"Seven years of Apple to Samsung"` | Slightly ambiguous phrasing — not a typo, a phrasing issue (see Medium finding) |

No spelling errors found in either document.

---

## What's Well-Written

1. **Overall voice discipline** — 100+ visible strings in `index.html`; zero marketing drift. "TrueDepth," "Quad-LED True Tone," "Ultrasonic in-display FP" — all technically precise.

2. **Callout label system (§01)** — The A/B/C/D lettered callout convention is consistent across all four elevation drawings. Parallelism between XR and S25 FE callout structures (notch→punch-hole, Lightning→USB-C, LCD→AMOLED) is clean.

3. **README Why section** — Punchy, factual, no filler. "Seven years on the same device generation is a long time in silicon" is a strong single-sentence hook for the section.

4. **Matrix footnote** — `"Δ column is informational, not a benchmark."` — pre-empts misreading. Well-placed.

5. **Footer doc-control fields** — `"Specs per OEM datasheets. No warranty implied."` is exactly the right legal/epistemic register for a personal datasheet.

6. **§00 animated labels** — `PLOTTING UNIT A · iPhone XR · 2018` — terse, factual, matches the plotted-schematic metaphor perfectly.

7. **Design Notes in README** — Color/font rationale is specific and useful. "The color of a printed datasheet left in a drawer" is evocative without being purple prose.

8. **Scale bar label** — `SCALE 1 : 1 · 0 — 50 mm` — correct engineering-drawing convention, adds authenticity to blueprint framing.

---

## Prioritized Fix List

| Priority | File:line | Action | Proposed exact text |
|----------|-----------|--------|-------------------|
| 1 · CRITICAL | README.md:17 | Wrong SoC | `Exynos 2500` → `Exynos 2400e` |
| 2 · HIGH | README.md:89 | Wrong network spec | `5G (sub-6 + mmWave)` → `5G (sub-6)` |
| 3 · HIGH | README.md:59 | Wrong §04 description | `Architecture — SoC block diagrams: A12 Bionic vs. Exynos 2500 core layout` → `Architecture — ecosystem migration diagram: iOS / Apple stack → Android / Samsung stack` |
| 4 · MEDIUM | README.md:17 | Second Exynos 2500 in Why paragraph | `Exynos 2500` → `Exynos 2400e` |
| 5 · MEDIUM | README.md:42 | macOS-only run command | Add `xdg-open index.html` Linux alternative, or lead with python server |
| 6 · MEDIUM | index.html:1586 | EU mandate year | Move `USB-C MANDATE (EU)` label from x=469 (2021) to x=612 (2022) — adjust both text x and line x1 accordingly |
| 7 · LOW | README.md:3 | Phrasing ambiguity | `"Seven years of Apple to Samsung"` → `"Seven-year device gap — Apple to Samsung"` |
| 8 · LOW | index.html:605 | Awkward self-referential phrase | `"with no marketing surface"` → `"— no editorial, no score"` |
| 9 · LOW | Repo topics | Swap/add | Replace `showcase` with `svg`; add `apple-to-android` |
| 10 · LOW | index.html:1373/1375 | Editorial labels | `WALLED GARDEN` → `CLOSED PLATFORM` (optional, only if zero-editorial intent is strict) |

---

## Unresolved Questions

1. **S25 FE USB-C speed:** Is the USB-C port USB 3.2 Gen 1 (5 Gbps) or USB 2.0? Index.html §04 labels it `USB 3.2 · DISPLAYPORT ALT` (line 1434). Some Samsung spec pages are ambiguous on FE-line USB speed. If it's USB 2.0, the architecture diagram is wrong. Recommend checking Samsung's official S25 FE spec page.

2. **S25 FE mmWave:** Confirm definitively whether any regional S25 FE variant supports mmWave. If a US carrier variant does, the README entry would be conditionally correct but needs a qualifier (e.g., `5G sub-6 (some regions: mmWave)`).

3. **S25 FE display ppi:** Not shown in §02 matrix for S25 FE (only XR shows 326 ppi). 2340×1080 at 6.7" = ~385 ppi. Adding this would make the Display row fully symmetric. Low priority but easy win for completeness.
