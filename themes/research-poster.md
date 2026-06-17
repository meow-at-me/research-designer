# research-poster.md

> Theme: pale-gray editorial poster canvas, zero-radius, hairline structure; solid-black header bars title every panel; one burnt-persimmon accent for key numbers; a stepped-block title banner. Reads in columns, not scroll.
> Archetype: research-poster.
> Use case: academic conference poster that renders on screen **and** prints to PDF at a fixed size (A0). Method/results summary at a glance.
> Base themes: theme-park.md `orange-offset-mono` (editorial zero-radius system, Black Header/Tab bars, burnt-persimmon accent) + `kinetic-type` (the web-only per-line title reveal technique only).
> Icons: Lucide (ISC), stroke 1.5px. No emoji — line-style SVG only.
> Technique note: a conference poster's rigid sectioned grid expressed in offset-mono's hairline editorial grammar; borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **0 — native**, web only. A poster is fundamentally static; motion is a thin web-only courtesy, so no JS motion library. |
| Libraries | none — native CSS + IntersectionObserver (bar grow) · `@page`/`@media print` |
| The motion idea | The whole poster renders static on load — it's one fixed canvas meant to be seen at once (screenshot- and print-safe; no scroll-reveal that would hide content). The only web motion: comparison bars grow from baseline once when scrolled into view (IntersectionObserver toggles a class). Nothing loops or autoplays. |
| The 3D idea | None — Tier 2 is excluded: the artifact must print. |
| Reduced-motion | Identical to the print render: title and bars render in final static state, no wipe, no grow. |
| Print behavior | `@page { size: A0 portrait; margin: 0 }` + `@media print` strips all web accents and renders the poster at true physical size for Print-to-PDF. |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | One artifact, two outputs | Same DOM renders to a scaled-to-fit canvas on screen and to a true A0 sheet on paper |
| 2 | Read in columns, glance in seconds | 3-column body; takeaways and headline numbers are oversized and accent-marked |
| 3 | Black bars title everything | Every panel/table/figure opens with a solid-black header/tab — the canonical poster title treatment |
| 4 | Hairline structure, zero radius | Separation by hairline rules + black bars + spacing, never shadows or rounded cards |
| 5 | Accent is data, not decoration | Burnt persimmon only on key numbers / one chart series / active state — ≤2 per panel |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--canvas` | #EDEDEF | Poster background (cool pale gray, also the gutter between panels) |
| `--band` | #FFFFFF | Panel fills |
| `--ink` | #111113 | Black bars, headings, primary text |
| `--ink-soft` | #55555A | Body copy |
| `--ink-faint` | #9A9AA0 | Captions, sources, meta |
| `--line` | #DDDDE1 | Hairline rules / borders |
| `--accent` | #E2603A | Burnt persimmon — key numbers, one chart series, active states |
| `--accent-soft` | #F6DED4 | Accent tint — tag fills, chart area fills |
| `--neutral-bar` | #D2D2D6 | Baseline series in comparison bars |

Rule: **zero radius on every box.** Accent never on body text or large fills; flat fills only, no gradients. Exemptions: chart dot markers (circles), QR module.

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| Poster title (banner) | Archivo | 800 / clamp(34px, 3.2vw, 64px) at 1× / -0.02em, stepped or single block, white on `--ink` |
| Panel heading (in black bar) | Inter Tight | 700 / 18–22px / white, 0.02em |
| Body | Inter | 400 / 15–16px / 1.55, `--ink-soft`, measure ≤ 60ch per column |
| Numeric emphasis | Archivo | 700–800 / 28–56px, key figures may use `--accent` |
| Caption / eyebrow / meta | JetBrains Mono | 11–12px uppercase, +0.06em, `--ink-faint` |

## 4. Layout

- **Canvas:** A0 portrait — `.poster` is `841mm × 1189mm` (aspect ≈ 0.707). On screen a `.stage` wraps it and applies `transform: scale(var(--fit))`, `--fit` computed by JS = viewport-fit, centered; on `@media print` the scale resets to 1 and `@page { size: A0 portrait }` prints it physically.
- **Title banner:** full-width top zone — stepped-block method title (Archivo 800, ≤3 lines), author row with superscript affiliation markers, affiliation legend, venue badge, optional institutional wordmark slot top-right (own-logo exception).
- **Body grid:** 3 equal columns, ~24mm gutter; panels stack within columns. A panel may span 2–3 columns (method diagram, results table).
- **Footer strip:** full-width bottom band — QR module (links to fictional project page) + arXiv id + the mandatory disclaimer, mono.
- Margins ~30mm. No content closer than the margin to the sheet edge (print-safe).

## 5. Components

| Component | Spec |
|---|---|
| Black Header Bar | `background:var(--ink); color:#fff;` Inter Tight 700, spans the panel width, square corners — opens every panel/table |
| Black Tab Label | small inline black tab top-left of every `<figure>`, mono/Inter 600, square |
| Panel | white fill, optional `1px solid var(--line)`; opens with a Black Header Bar; zero radius |
| Contributions box | white panel, `border-left: 3px solid var(--accent)`, bulleted contributions |
| Comparison bar figure | titled by Black Tab; `--neutral-bar` baselines vs `--accent` ours; value labels Archivo 700 above; axis-free; grows from baseline once on scroll-in |
| Quantitative table | Black Header Bar; hairline row dividers; mono tabular numerals; best row/cell bold + `--accent` |
| Method diagram | self-made inline SVG pipeline (input views → encoder → 3D field → render), labeled, no external image |
| Takeaway / number stat | oversized Archivo numeral (`28.41 dB` in `--accent`) + small mono label beneath |
| Tag | square, `--accent-soft` fill, `--ink` 12px |
| QR module | self-generated square matrix (CSS grid of cells) or self-made SVG — links to the fictional project URL |

## 5b. Genre components — REQUIRED

A poster demo missing any of these fails the spec.

| # | Required block | Notes |
|---|---|---|
| 1 | Title banner | method title + author row + affiliation legend + venue badge (PURRCV 2026) + wordmark slot |
| 2 | Introduction / motivation | the problem, in 3–5 sentences |
| 3 | Contributions box | 3–4 bulleted contributions, accent-marked |
| 4 | Method diagram | inline SVG architecture/pipeline, spanning ≥2 columns |
| 5 | Qualitative results | figure strip of synthetic renders (CSS/SVG stand-ins) under a Black Tab |
| 6 | Quantitative comparison table | WhiskerSplat vs ≥4 baselines, best bold, under a Black Header Bar |
| 7 | Ablation table or figure | ≥1 ablation (view count / component removal) |
| 8 | Conclusion / future work | short |
| 9 | References | compact, incl. `park2026whiskersplat`; ≥2 fictional prior works |
| 10 | Footer strip | QR to project page + arXiv id + mandatory disclaimer |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| Whole poster | static on load — no entrance (one fixed canvas, screenshot- & print-safe) | — |
| Comparison bars | grow from baseline, 600ms, once on scroll-into-view (web only) | 0 / IntersectionObserver toggles a class |
| Print / reduced motion | everything in final static state; no grow | — |

## 7. 3D

Not applicable — Tier 2 excluded (the poster must print). State this explicitly in §0.

## 8. Don't

- No rounded corners, no drop shadows, no card lift — hairlines + black bars + spacing only.
- No plain-text panel titles — every panel/table/figure opens with a black bar/tab.
- No accent on body text or large fills; ≤2 accent elements per panel.
- No looping/ambient motion; web accents are one-shot and absent in print.
- No external images — figures are self-made inline SVG/CSS; QR is self-generated.
- No real venue/dataset/author/metric — fictional canon only, all numbers labeled fictional.
- Don't let content cross the print margin; keep the A0 aspect intact on screen.

---

## CSS tokens

```css
:root{
  --canvas:#EDEDEF; --band:#FFFFFF; --ink:#111113; --ink-soft:#55555A; --ink-faint:#9A9AA0;
  --line:#DDDDE1; --accent:#E2603A; --accent-soft:#F6DED4; --neutral-bar:#D2D2D6;
  --radius:0;
  --poster-w:841mm; --poster-h:1189mm; --fit:1;   /* --fit set by JS = viewport-fit scale */
  --font-display:"Archivo",sans-serif; --font-head:"Inter Tight",sans-serif;
  --font-body:"Inter",system-ui,sans-serif; --font-mono:"JetBrains Mono",monospace;
}
@page{ size: A0 portrait; margin: 0; }
```
