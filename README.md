# Device Evolution // iPhone XR → Galaxy S25 FE

Personal upgrade datasheet. Seven years of Apple to Samsung, rendered as a technical blueprint.

---

## What

A single-page upgrade record documenting the hardware delta between an iPhone XR (Oct 2018) and a Samsung Galaxy S25 FE (2025). Not a review. Not a recommendation. A datasheet — the kind you'd produce if you were filing a component change order and needed to justify every dimension that changed.

Rendered as one static `index.html`. No framework, no build pipeline, no server. Open it and it works.

---

## Why

Seven years on the same device generation is a long time in silicon. The XR shipped with a 7nm A12, a single 12MP camera, 3GB of RAM, and an LCD panel. The S25 FE ships with a 4nm Exynos 2500, a 50MP triple-array, 8GB of RAM, and a 120Hz AMOLED panel. The ecosystem also flipped: Lightning → USB-C, iOS → Android, Apple Silicon → Arm Cortex-X.

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

The entire page ships as one file. Total external requests: 1 (Google Fonts CDN).

---

## Run Locally

```bash
open index.html
```

Or, if your browser blocks `file://` for font loading:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

---

## Sections

- **Elevations** — front/side/back orthographic outlines of both devices, annotated with key dimensions
- **Spec Matrix** — side-by-side table across 20+ hardware fields
- **Capability Deltas** — visual delta bars showing magnitude of change per category
- **Architecture** — SoC block diagrams: A12 Bionic vs. Exynos 2500 core layout
- **Timeline** — annotated product-line timeline from 2018 to 2025, marking the upgrade point

---

## Design Notes

The aesthetic is engineering blueprint, not consumer tech:

- **Paper**: warm off-white `#F5F0E8` — the color of a printed datasheet left in a drawer
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
| Network | 4G LTE | 5G (sub-6 + mmWave) |

---

## License

Apache-2.0. See [LICENSE](LICENSE).

---

## Author

[tiennm99](https://github.com/tiennm99)

---

*Built 2026-05-17.*
