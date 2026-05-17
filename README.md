# Device Evolution // iPhone XR → Galaxy S25 FE

Personal upgrade datasheet — seven-year device gap, Apple to Samsung, rendered as a technical blueprint.

---

## What

A personal upgrade record documenting the hardware delta between an iPhone XR (Oct 2018) and a Samsung Galaxy S25 FE (2025). Not a review. Not a recommendation. A datasheet — the kind you'd produce if you were filing a component change order and needed to justify every dimension that changed.

Rendered **six different ways**. Same facts, same SVG geometry — six document traditions. `index.html` is a selector that opens any of the six. Each format is a self-contained static HTML file. No framework, no build pipeline, no server.

---

## Why

Seven years on the same device generation is a long time in silicon. The XR shipped with a 7nm A12, a single 12MP camera, 3GB of RAM, and an LCD panel. The S25 FE ships with a 4nm Exynos 2400e, a 50MP triple-array, 8GB of RAM, and a 120Hz AMOLED panel. The ecosystem also flipped: Lightning → USB-C, iOS → Android, Apple custom cores → ARM Cortex reference cores.

The page exists to map those deltas precisely — not to editorialize about which is better.

---

## Stack

| Layer | Choice |
|-------|--------|
| Markup | Semantic HTML5 |
| Style | Vanilla CSS, inline `<style>` block |
| Graphics | Inline SVG (no external image files) |
| Fonts | Google Fonts — Inter + JetBrains Mono (loaded via `<link>`) |
| JS | None |
| Build | None |
| Dependencies | None |

Each format is one file. Total external requests per page: 1 (Google Fonts CDN) or 0 (the Apple-style formats lean on system SF Pro and ship without web fonts).

---

## Run Locally

```bash
# macOS
open index.html

# Linux
xdg-open index.html
```

Or, if your browser blocks `file://` for font loading:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

---

## Formats

| File | Format | Archetype |
|------|--------|-----------|
| `index.html` | Selector hub | — |
| `blueprint.html` | Engineering datasheet | architectural elevation drawing (origin) |
| `cupertino.html` | Cupertino Edition | apple developer-docs / HIG |
| `newsroom.html` | Newsroom Long-Read | apple.com/newsroom editorial |
| `keynote.html` | Keynote Spec Sheet | apple product page (full-bleed long-scroll) |
| `compare.html` | Compare-Page Purist | apple.com/shop/buy-iphone/compare |
| `diptych.html` | Two-Tone Diptych | vertical split, 2018-vs-2025 |

Each format contains the same five logical sections — Elevations, Spec Matrix, Capability Deltas, Architecture, Timeline — restyled per its source tradition. The §00 plot-erase-replot animation is preserved in every format except `compare.html` (compare pages don't animate) and demoted in `newsroom.html` (single article-figure).

---

## Design Notes

The aesthetic is engineering blueprint, not consumer tech:

- **Paper**: warm off-white `#FAFAF7` — the color of a printed datasheet left in a drawer
- **Ink**: near-black `#1A1A1A` — not pure black, avoids harshness on warm paper
- **Accent**: single steel-blue `#3D5A6C` — used for callout lines, dimension arrows, section headers
- **Grid**: faint blueprint grid underlays the page at low opacity
- **Type**: Inter for prose labels, JetBrains Mono for all spec values and callout text
- **Dimension style**: numbers formatted as engineering callouts with leader lines, not tooltip popovers
- No neon. No LED glow. No RGB gradients. No dark mode.

The constraint is intentional: one accent color forces visual hierarchy through weight and size, not hue.

---

## Spec Deltas at a Glance

| Dimension | iPhone XR (2018) | Galaxy S25 FE (2025) |
|-----------|-----------------|----------------------|
| Display | 6.1" LCD, 60Hz | 6.7" AMOLED, 120Hz |
| RAM | 3 GB | 8 GB |
| Battery | 2942 mAh | 4900 mAh |
| Charging | 18W Lightning | 45W USB-C |
| Cameras | 1 (12MP wide) | 3 (50MP + 12MP + 8MP) |
| Network | 4G LTE | 5G (sub-6) |

---

## License

Apache-2.0. See [LICENSE](LICENSE).

---

## Author

[tiennm99](https://github.com/tiennm99)

---

*Built 2026-05-17.*
