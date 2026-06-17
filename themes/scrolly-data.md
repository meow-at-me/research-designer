# scrolly-data.md

> Theme: white reading canvas, serif body, scroll-driven charts — a data essay where graphics advance as prose scrolls.
> Use case: long-form articles, data reports, research write-ups, annual reviews.
> Icons: Lucide (ISC), stroke 1.5px. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Prose drives, graphics respond | Sticky chart panel + scrolling text steps; each step mutates the chart |
| 2 | Color belongs to data | Page is monochrome; only chart marks and text highlights carry color |
| 3 | Reading-first measure | One narrow text column; charts may break wide |
| 4 | Numbered, archival tone | Mono issue/figure numbers stamp every section |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | #FFFFFF | Page |
| `--bg-warm` | #F7F4ED | Alternate sections, figure mats |
| `--text` | #242424 | Body |
| `--text-soft` | #6B6B6B | Captions, secondary |
| `--border` | #D9D6CE | 1px rules, card borders |
| `--accent` | #1E7F3C | Links, active step (generic editorial green) |
| `--mark` | #F2DD4E | Inline text highlight (`<mark>`) |
| `--button-bg` | #242424 | Primary buttons, white text |

Chart series (colorblind-safe categorical set — use in this order):
`#4477AA` blue · `#EE6677` red · `#228833` green · `#CCBB44` yellow · `#66CCEE` cyan · `#AA3377` purple · `#BBBBBB` gray

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Display / headlines | Archivo (SIL OFL) | h1 48px / 700, h2 36px, h3 28px, h4 24px; tight 1.15 |
| Body | Lora (SIL OFL) | 20px / 1.6 — body is serif, always |
| UI, captions, axis labels | Archivo | 13-14px / 500 |
| Figure numbers, data callouts | JetBrains Mono (SIL OFL) | 12-13px, uppercase, 0.05em |

Size tokens (fixed): 12 / 14 / 16 / 18 / 20 / 24 / 28 / 36 / 48 / 64.
Inline highlight: `<mark>` with `--mark` bg, no radius, 0 2px padding.

## 4. Layout

- Text measure: 680px center column; charts/figures may extend to 1080px ("breakout")
- Scrolly section: graphic `position: sticky; top: 0; height: 100vh` + text steps in a 320-380px column overlapping or beside it; each step card triggers one chart state
- Steps: white card, 1px `--border`, padding 24px, margin 70vh between steps (one state per viewport)
- Header: thin top progress bar (3px, `--accent`, scroll-linked width)
- Footer/figure mats: `--bg-warm` full-bleed bands
- Section stamp: mono number chip (e.g. `FIG. 04`) above every headline

## 5. Radius Scale (fixed — never freehand)

| Token | Value | Used on |
|---|---|---|
| xs | 2px | Chips, marks |
| sm | 4px | Buttons, inputs |
| md | 8px | Step cards, tooltips |
| lg | 16px | Poster cards, large figures |

## 6. Components

| Component | Spec |
|---|---|
| Primary button | `--button-bg`, white text, radius 4px, height 44px, Archivo 14px/600 |
| Issue chip | Mono 12px, 1px `--border`, radius 2px, 2px 8px padding |
| Step card | White, 1px `--border`, radius 8px; active: border-left 3px `--accent` |
| Chart frame | No box; hairline x/y axes `--border`; direct series labels (no legend when ≤4 series) |
| Big number | Mono, 64px, one per section max; caption beneath in 14px `--text-soft` |
| Poster card (index pages) | Radius 16px, 1px `--border`; each card may carry its own flat poster bg color |
| Tooltip | #242424 bg, white 13px text, radius 4px, no shadow |

## 7. Motion

Root duration variable: `--1s: 1ms;` overridden to `1s` only under `prefers-reduced-motion: no-preference`. Every duration below is written as `calc(var(--1s) * n)` — reduced motion collapses all animation automatically.

| Target | Effect |
|---|---|
| Chart state change | 0.6s ease-in-out attribute/position transitions |
| Step activation | Border + text color 0.2s; inactive steps at 0.55 opacity |
| Big number | Count-up once on first reveal, 0.8s |
| Progress bar | Scroll-linked, no easing |

## 8. Don't

- No gradients, shadows, or dark sections — depth comes from `--bg-warm` bands and hairlines
- Chart series colors only in the listed order; never decorate UI chrome with them
- No legend when direct labeling fits; never more than 7 series
- Body text never in sans; UI text never in serif
- No parallax or text movement during scrolly — only the graphic animates


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#FFFFFF; --bg-warm:#F7F4ED; --text:#242424; --text-soft:#6B6B6B;
  --border:#D9D6CE; --accent:#1E7F3C; --mark:#F2DD4E; --button-bg:#242424;
  /* colorblind-safe categorical series (fixed order) */
  --s1:#4477AA; --s2:#EE6677; --s3:#228833; --s4:#CCBB44; --s5:#66CCEE; --s6:#AA3377; --s7:#BBBBBB;
  --radius-xs:2px; --radius-sm:4px; --radius-md:8px; --radius-lg:16px;
  --font-display:"Archivo",sans-serif; --font-body:"Lora",serif; --font-mono:"JetBrains Mono",monospace;
}
```
