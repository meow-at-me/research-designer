# research-project-page.md

> Theme: white reading canvas, serif abstract, hairline structure, mono figure stamps; one serif-italic accent in the title and an indigo link row. The hero teaser is a *real* interactive 3D reconstruction the reader can drag.
> Archetype: research-project-page.
> Use case: academic paper / project landing page — title, authors, abstract, teaser, method, qualitative + quantitative results, ablations, BibTeX.
> Base themes: theme-park.md `scrolly-data` (white data-essay canvas, colorblind-safe series, mono figure stamps, count-up numbers) + `editorial-hairline` (serif-italic title accent, indigo links/focus, hairline rules, static-poise opacity-only entrance).
> Icons: Lucide (ISC), stroke 1.5px. No emoji — line-style SVG only.
> Technique note: the canonical "project page" genre rendered in a restrained data-essay system, with the repo's real-3D ethos in the teaser. Borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **2 — WebGL** (interactive point-cloud reconstruction viewer in the teaser) |
| Libraries | three.js + OrbitControls (3D viewer) · Motion (metric count-up + figure reveals) · native CSS scroll-driven (progress bar, before/after slider) |
| The motion idea | The teaser is a draggable 3D point cloud (a stylized reconstructed scene); headline metrics count up once; figures reveal on scroll; a before/after slider wipes rendered vs ground-truth |
| The 3D idea | A procedurally generated colored point cloud (≈30–60k points, additive splat-like sprites) standing in for a 3D Gaussian/NeRF reconstruction; OrbitControls drag-to-rotate, **no autorotate** (screenshot-ready); static fallback = a self-made false-color depth poster |
| Reduced-motion | Autorotate off (already), figure reveals + count-ups instant, smooth-scroll off; the slider stays usable (input-driven, not animated) |
| WebGL fallback | Feature-detect; on no-WebGL show a static false-color depth-map SVG/CSS poster + caption "Interactive 3D requires WebGL." DPR ≤2, pause loop on `document.hidden` |
| Dark mode | Light + dark via CSS-variable token sets; **follow `prefers-color-scheme` by default**, with a manual toggle that overrides (persisted, applied before paint to avoid flash). Diagram figures stay on a fixed "lit" light card so they read in dark; the 3D viewer panel is dark in both. |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Genre first | Every block a reviewer expects is present and in order (see §5b); aesthetics never displace content |
| 2 | Color belongs to data | Page is near-monochrome; chart marks + one serif-italic accent + indigo links carry the only color |
| 3 | Reading-first measure | Narrow prose column (Lora serif); figures, tables, and the teaser break wide |
| 4 | The teaser is honest | The 3D is a real draggable scene, not a looping video of one — that is the repo's reason to exist |
| 5 | Static poise | Entrances are opacity-only; the hero renders complete on load |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | #FFFFFF | Page |
| `--bg-warm` | #F7F4ED | Figure mats, alternate bands, BibTeX block |
| `--text` | #242424 | Body |
| `--text-soft` | #6B6B6B | Captions, affiliations, meta |
| `--border` | #D9D6CE | Hairline rules, table/figure borders |
| `--accent` | #5266EB | Links, link buttons, focus ring, active state (indigo) |
| `--mark` | #F2DD4E | Inline `<mark>` highlight (sparingly) |
| `--ink-btn` | #242424 | Primary buttons / "best" emphasis, white text |

Chart / qualitative series (colorblind-safe, fixed order): `#4477AA · #EE6677 · #228833 · #CCBB44 · #66CCEE · #AA3377 · #BBBBBB`.
Depth/heat ramp (false-color fallback + comparisons): perceptual blue→teal→yellow, e.g. `#2A2F6B → #1F8F86 → #CCBB44`.

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| Title | Archivo | clamp(34px, 5vw, 60px) / 700 / -0.02em / 1.1 |
| Title accent | Instrument Serif Italic | one phrase inside the title only (e.g. *Sparse Views*); ≤1 instance |
| Section heading | Archivo | h2 32px / h3 24px / 700 |
| Abstract & prose | Lora | 20px / 1.6 — body is serif |
| UI / captions / table cells | Archivo | 13–15px / 500 |
| Figure stamps, metric callouts, BibTeX, mono labels | JetBrains Mono | 12–13px, uppercase for stamps, +0.05em; tabular-nums in tables |

## 4. Layout

- Prose measure 680px centered; figures/tables/teaser break to 1080px.
- Top: 3px scroll-linked progress bar (`--accent`).
- **Title block** (centered): venue badge → title (with serif-italic accent) → author row → affiliation legend → corresponding email → link-button row.
- **Teaser** (full breakout): the 3D viewer canvas with a thin hairline frame + mono caption stamp; a small "drag to rotate" hint chip.
- **Headline metrics strip**: 3–4 big mono numbers (count-up) directly under the teaser.
- Then in order: Abstract → TL;DR/Contributions → Method figure → Before/after slider → Qualitative gallery → Quantitative table → Ablations → BibTeX → Acknowledgements → Footer.
- Figure mats and the BibTeX block sit on `--bg-warm` full-bleed bands; sections separated by whitespace + hairlines, never shadows.

## 5. Components

| Component | Spec |
|---|---|
| Venue badge | uppercase mono 11px, hairline-border pill, e.g. `PURRCV 2026 · ORAL` |
| Author row | names in Archivo 16px, superscript affiliation markers (`¹ ²`), corresponding author marked; wraps centered |
| Link button | hairline-border rect, radius 4px, Lucide icon + label (Paper, arXiv, Code, Video, Dataset, BibTeX); hover → `--accent` border+text |
| 3D viewer frame | breakout canvas, 1px `--border`, mono caption stamp top-left, drag hint bottom-right |
| Big metric | Mono 48–64px, count-up once; small `--text-soft` label beneath |
| Method figure | self-made inline SVG pipeline (input views → encoder → 3D field → differentiable render), labeled |
| Before/after slider | two stacked plates (rendered / ground-truth) + a draggable divider via range input + `clip-path`; labels A/B |
| Qualitative gallery | grid of scene cards (self-made false-color/point-field plates); hover swaps view/baseline |
| Quantitative table | hairline rows, mono tabular numerals, header row; **best per column bold + `--accent`**; ↑/↓ arrow marks which is better |
| Ablation table | compact variant of the quantitative table |
| BibTeX block | `--bg-warm` mat, mono, copy-to-clipboard button (Lucide `clipboard`) |
| Tooltip | `#242424` bg, white 13px, radius 4px, no shadow |

## 5b. Genre components — REQUIRED

| # | Required block | Notes |
|---|---|---|
| 1 | Title + venue badge | full method title, serif-italic accent on one phrase, PURRCV 2026 badge |
| 2 | Author + affiliation row | superscript markers + affiliation legend + corresponding-author email |
| 3 | Link-button row | Paper · arXiv · Code · Video · Dataset · BibTeX (Lucide icons) |
| 4 | Teaser | the interactive 3D viewer (or its static fallback) |
| 5 | Abstract | single narrow serif column |
| 6 | TL;DR / contributions | bulleted, 3–4 items |
| 7 | Method figure | inline SVG architecture/pipeline diagram |
| 8 | Before/after comparison | slider over rendered vs ground-truth |
| 9 | Qualitative results gallery | ≥4 scenes |
| 10 | Quantitative comparison table | WhiskerSplat vs ≥4 baselines, best bold |
| 11 | Ablation table | ≥1 ablation (view count / component) |
| 12 | BibTeX block | copy button |
| 13 | Acknowledgements | short, fictional |
| 14 | Footer disclaimer | mandatory text |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| 3D teaser | drag-to-rotate, gentle inertia; no autorotate | 2 / three.js + OrbitControls |
| Headline metrics | count-up once on first reveal, 0.8s | 1 / Motion |
| Figures / sections | opacity-only fade-in on scroll (static poise) | 0 / `view()` (+ IO fallback) |
| Before/after slider | divider follows input; `clip-path` reveal | 0 / range input, no JS animation |
| Progress bar | scroll-linked scaleX | 0 / `scroll(root)` (+ JS fallback) |
| Reduced motion | no count-up/fade/smooth-scroll; slider + drag still work | — |

## 7. 3D (Tier 2)

| Aspect | Spec |
|---|---|
| Scene / object | procedural colored point cloud (~30–60k `Points`), shaped as a small interior scene; per-point color from a self-made palette; additive/soft sprite for a splat look |
| Material / shading | `PointsMaterial` (or a small custom shader) with size-attenuation; subtle depth-fade; no external textures |
| Camera / interaction | `PerspectiveCamera` + `OrbitControls`, damping on, **autoRotate off**; one static framed view on load |
| Performance | DPR cap ≤2; pause render loop on `document.hidden`; cap point count on coarse pointers / small viewports |
| Fallback | static false-color depth-map poster (self-made SVG/CSS) + caption when WebGL unavailable |

## 8. Don't

- No missing genre block (§5b) — a project page without authors/abstract/BibTeX fails the spec.
- No looping autoplay teaser pretending to be interactive — the 3D must actually be draggable, or show the honest static fallback.
- No gradients/shadows/dark hero — depth from `--bg-warm` mats + hairlines.
- No body text in sans, no UI text in serif; chart series colors only in the listed order, never on chrome.
- No real method/dataset/venue/author/metric — fictional canon only; all numbers labeled fictional.
- No accent beyond links/focus/best-cell and the single serif-italic title phrase.

---

## CSS tokens

```css
:root{
  --bg:#FFFFFF; --bg-warm:#F7F4ED; --text:#242424; --text-soft:#6B6B6B; --border:#D9D6CE;
  --accent:#5266EB; --mark:#F2DD4E; --ink-btn:#242424;
  /* colorblind-safe categorical series (fixed order) */
  --s1:#4477AA; --s2:#EE6677; --s3:#228833; --s4:#CCBB44; --s5:#66CCEE; --s6:#AA3377; --s7:#BBBBBB;
  --radius-xs:2px; --radius-sm:4px; --radius-md:8px; --radius-lg:16px;
  --font-display:"Archivo",sans-serif; --font-serif-accent:"Instrument Serif",serif;
  --font-body:"Lora",serif; --font-mono:"JetBrains Mono",monospace; --font-ui:"Archivo",sans-serif;
}
```
