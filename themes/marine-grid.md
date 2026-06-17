# marine-grid.md

> Theme: square-corner corporate sensor board — flat white surfaces, hairline borders, one deep-marine blue family, oversized numerals.
> Use case: corporate monitoring consoles, engineering status boards (light default, "midnight marine" dark variant).
> Icons: Lucide (ISC), stroke 1.75px, 20×20, round caps. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Square geometry | `border-radius: 0` everywhere (status chips max 2px); depth from hairlines, never shadows |
| 2 | One blue family | Monochrome-leaning: a deep brand blue + an active blue carry the whole page; no rainbow |
| 3 | Brand-site polish on engineering data | Oversized numerals, uppercase micro-labels (+0.12em), restrained motion |
| 4 | Hairline as signature | Every panel opens with a 2px accent hairline drawing across its top edge |

## 2. Color Tokens

### 2.1 Light (default)

| Token | Hex | Use |
|---|---|---|
| `--bg` | #F4F4F4 | App background |
| `--bg-panel` | #FFFFFF | Panel surface |
| `--bg-raised` | #F4F4F4 | Hover rows, inputs |
| `--line` | #E0E0E0 | Hairlines, gridlines |
| `--ink` | #161616 | Primary text, big numerals |
| `--text-dim` | #6F6F6F | Labels, ticks |
| `--brand` | #003D8F | Deep marine — panel labels, wordmark |
| `--accent` | #0F62FE | Active states, focus, primary series, top hairline |

### 2.2 Dark ("midnight marine")

| Token | Hex | Use |
|---|---|---|
| `--bg` | #061224 | App background |
| `--bg-panel` | #0B1F3A | Panel surface |
| `--bg-raised` | #102747 | Hover rows, inputs |
| `--line` | #24395C | Hairlines, gridlines |
| `--ink` | #F2F4F8 | Primary text, numerals |
| `--text-dim` | #9FB1C9 | Labels, ticks |
| `--brand` | #78A9FF | Panel labels, wordmark |
| `--accent` | #4589FF | Active states, focus, primary series |

### 2.3 Data series (blue family — fixed order, per-theme)

| Index | Light | Dark |
|---|---|---|
| s1 | #0F62FE | #4589FF |
| s2 | #1192E8 | #33B1FF |
| s3 | #007D79 | #08BDBA |
| s4 | #6929C4 | #A56EFF |
| s5 | #161616 | #F2F4F8 |

Status: ok #198038 / #42BE65 · warn #B28600 / #D2A106 · crit #DA1E28 / #FA4D56 (light / dark).

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Display (big stats) | Figtree (SIL OFL) | 30px / 700 / -0.01em, tabular-nums |
| Panel label | Figtree | 11px / 600, +0.12em, uppercase, `--brand` |
| Body, legend, table | Figtree | 13px / 400 |
| Axis ticks | Figtree | 11px / 500, tabular-nums |

No monospace — numerals stabilized with `tabular-nums`.

## 4. Layout

- Sidebar (collapsible) 236px / 58px, 250ms ease, auto-collapse under 920px; square nav items, 2px inset accent on active; hairline right border; square `--brand` logo mark.
- Topbar 56px: context id · range · refresh · theme.
- Body: KPI strip of 5 flat tiles (delta below value) → trends line (8) + bars (4) → area (6) + ranked list (6) → status table (12).
- Grid 12 col, gutter 12px, panel padding 18px, **radius 0**; panel = 1px `--line`, no shadow; 2px `--accent` top hairline.
- Buttons/selects square; table rows ruled by hairlines only.

## 5. Charts (mandatory)

- Ticks 1-2-5 nice steps; 4–5 y, 5–8 x; y-domain `[niceFloor(min), niceCeil(max)]`; max never clipped.
- Both axes labeled; SI-abbreviated numbers; time `HH:MM`; unit stated once in panel caption.
- Hover: crosshair + tooltip with square swatches, nearest-x snap, edge flipping.
- Legend under chart (square dot + label); click toggles (off = 35%), axes recompute; hover dims others to 22%.
- Gridlines horizontal only; no plot border except baseline axis.

## 6. Motion

| Step | Spec |
|---|---|
| Page enter | Panels rise 16px + fade, stagger 50ms, 500ms cubic-bezier(.22,1,.36,1) |
| Panel hairline | 2px top edge draws scaleX(0→1) 600ms as each panel lands |
| Stat numbers | Count-up 750ms ease-out; delta fades in 120ms after |
| Lines | stroke-dashoffset 850ms; bars scale-Y stagger 26ms; list bars scale-X stagger 60ms |
| Hover | Instant |
| Theme switch | 250ms crossfade |
| Reduced motion | No entrances; final state rendered |

## 7. Don't

- No border-radius (chips 2px max), no shadows, no gradients
- Never more than the blue-family series; s5 ink is a reference line, not a category
- No second accent hue; warm colors appear only as status
- Don't mix mono fonts in — tabular-nums does the alignment
- Delta indicators always arrows + text, never color alone


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#F4F4F4; --bg-panel:#FFFFFF; --bg-raised:#F4F4F4; --line:#E0E0E0;
  --ink:#161616; --text-dim:#6F6F6F; --brand:#003D8F; --accent:#0F62FE;
  --s1:#0F62FE; --s2:#1192E8; --s3:#007D79; --s4:#6929C4; --s5:#161616;
  --ok:#198038; --warn:#B28600; --crit:#DA1E28;
  --radius:0; --radius-chip:2px; --space-unit:12px;
  --font-sans:"Figtree",system-ui,sans-serif;
}
[data-theme="dark"]{
  --bg:#061224; --bg-panel:#0B1F3A; --bg-raised:#102747; --line:#24395C;
  --ink:#F2F4F8; --text-dim:#9FB1C9; --brand:#78A9FF; --accent:#4589FF;
  --s1:#4589FF; --s2:#33B1FF; --s3:#08BDBA; --s4:#A56EFF; --s5:#F2F4F8;
  --ok:#42BE65; --warn:#D2A106; --crit:#FA4D56;
}
```
