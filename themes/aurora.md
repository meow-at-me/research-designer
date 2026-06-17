# aurora.md

> Theme: light-first analytics dashboard — airy rounded cards, one purple accent, a dark sidebar, donut/bar/line data viz.
> Use case: analytics consoles, reporting dashboards, admin panels (light default, dark variant included).
> Icons: Lucide (ISC), stroke 1.6px, 20×20, round caps. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Friendly, airy SaaS | Medium density; cards breathe on 16px gutters; confident but limited color |
| 2 | Chrome is calm, data is colorful | Surfaces near-neutral; saturated color only inside charts and the single accent |
| 3 | Always-dark sidebar | Navigation rail stays dark in both themes, anchoring the light canvas |
| 4 | Color is never the only signal | Series carry name + value in tooltips/legends, not hue alone (colorblind-safe set) |

## 2. Color Tokens

### 2.1 Light (default)

| Token | Hex | Use |
|---|---|---|
| `--bg` | #F7F8FC | App background |
| `--bg-panel` | #FFFFFF | Cards |
| `--bg-side` | #191B2B | Sidebar (always dark) |
| `--line` | #E4E7F2 | Borders, gridlines |
| `--text` | #1C2033 | Primary text |
| `--text-dim` | #6B7290 | Labels, ticks |
| `--accent` | #6929C4 | Primary actions, active nav |
| `--accent-soft` | #F1EBFB | Active nav bg, chips |

### 2.2 Dark

| Token | Hex | Use |
|---|---|---|
| `--bg` | #13141F | App background |
| `--bg-panel` | #1C1E2E | Cards |
| `--bg-side` | #101120 | Sidebar |
| `--line` | #2C2F45 | Borders, gridlines |
| `--text` | #EDEEF7 | Primary text |
| `--text-dim` | #9AA0BF | Labels, ticks |
| `--accent` | #BE95FF | Actions, active nav |
| `--accent-soft` | #2A2342 | Active nav bg, chips |

### 2.3 Data series (categorical, colorblind-tested — fixed order)

`#6929C4` · `#1192E8` · `#005D5D` · `#9F1853` · `#FA4D56` · `#570408` · `#198038` · `#B28600`

Status: success `#198038`, failed `#FA4D56`, inconclusive `#878D96`.

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Page title | Figtree (SIL OFL) | 22px / 700 |
| Card title | Figtree | 14px / 600 |
| Body, legend | Figtree | 13px / 400 |
| Big metric | Figtree | 30px / 700, tabular-nums |
| Axis ticks | Figtree | 11px / 500, tabular-nums |

## 4. Layout

- Sidebar (collapsible) 232px expanded / 64px collapsed; 250ms width ease; labels fade out collapsed, icons keep `title` tooltips; collapses by default under 920px. Holds logo mark + section labels (`11px uppercase +0.08em`) + nav with active highlight + footer (operator + version).
- Topbar: context select · search · avatar.
- Body: KPI row of 4 metric cards (delta chips) → row of bars + line → row of 3 donut cards.
- Card radius 14px; shadow (light only) `0 1px 2px rgba(28,32,51,.06), 0 8px 24px rgba(28,32,51,.06)`.

## 5. Components

| Component | Spec |
|---|---|
| Metric card | Big metric + label + delta chip (▲/▼ + value); radius 14px |
| Delta chip | Pill, success/failed tint at ~12% alpha + matching text |
| Donut card | Donut + right legend (swatch · label · value · %); click toggles a segment and recomputes % |
| Nav item | Icon + label; active: `--accent-soft` bg + `--accent` text |
| Skeleton | Shimmer blocks, 1.1s loop, until data binds |
| Focus ring | 2px `--accent` |

## 6. Charts (mandatory)

- Ticks: 1-2-5 nice steps; bars start at y=0; lines use padded data range; 4–5 y ticks.
- Hover: bars lighten +8% with tooltip; donut segment scales out 4px and center text swaps to hovered label + %; line crosshair + dot.
- Legend right of each donut; legend hover highlights the segment.

## 7. Motion

| Step | Spec |
|---|---|
| Page enter | Sidebar slides from left 240ms; cards fade-rise stagger 60ms, 500ms cubic-bezier(.22,1,.36,1) |
| KPI values | Count-up 650ms; delta chip pops scale .8→1 at 400ms |
| Bars | Grow from baseline, stagger 18ms, overshoot 1.04 then settle |
| Donuts | Conic sweep 0→360° 700ms ease-out, sequential 120ms apart |
| Hover | 140ms ease-out transforms only (no layout shift) |
| Reduced motion | Entrances off; donuts render complete |

## 8. Radius Scale (fixed — never freehand)

| Token | Value | Used on |
|---|---|---|
| chip | 999px | Delta chips, pills |
| card | 14px | Cards, panels |
| control | 8px | Buttons, inputs |

## 9. Don't

- Sidebar is never light — it stays dark in both themes
- Saturated color never on chrome; reserve it for charts + the single accent
- No more than the listed series colors, always in fixed order
- Never signal status by color alone — pair with text/value
- No gradients on cards; shadow appears in light theme only


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#F7F8FC; --bg-panel:#FFFFFF; --bg-side:#191B2B; --line:#E4E7F2;
  --text:#1C2033; --text-dim:#6B7290; --accent:#6929C4; --accent-soft:#F1EBFB;
  /* data series (categorical, fixed order) */
  --s1:#6929C4; --s2:#1192E8; --s3:#005D5D; --s4:#9F1853;
  --s5:#FA4D56; --s6:#570408; --s7:#198038; --s8:#B28600;
  --success:#198038; --failed:#FA4D56; --inconclusive:#878D96;
  --radius-chip:999px; --radius-card:14px; --radius-control:8px;
  --font-sans:"Figtree",system-ui,sans-serif;
}
[data-theme="dark"]{
  --bg:#13141F; --bg-panel:#1C1E2E; --bg-side:#101120; --line:#2C2F45;
  --text:#EDEEF7; --text-dim:#9AA0BF; --accent:#BE95FF; --accent-soft:#2A2342;
}
```
