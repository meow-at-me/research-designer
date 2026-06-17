# graphite.md

> Theme: matte industrial console — pure-neutral charcoal surfaces (equal R=G=B), all color reserved for data, ink-in reveal.
> Use case: machine/sensor monitoring, ops consoles, fleet dashboards (dark default, light "concrete" variant).
> Icons: Lucide (ISC), stroke 1.75px, 20×20, round caps/joins. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Machined metal, zero tint | Every surface is a pure neutral (R=G=B ±2) so UI never colors the data |
| 2 | Color belongs to series | Functional accents appear only in charts, status, focus — never on panels |
| 3 | Medium-high density | Icon rail + 12-col grid, 10px gutters, 14px panel padding |
| 4 | Ink-in reveal | Panels enter grayscale, then data colors saturate 350ms after lines finish drawing |

## 2. Color Tokens

### 2.1 Dark (default)

| Token | Hex | Use |
|---|---|---|
| `--bg` | #161618 | App background |
| `--bg-panel` | #1D1D20 | Panel surface |
| `--bg-raised` | #26262A | Hover rows, inputs, rail |
| `--line` | #313136 | Borders, gridlines |
| `--text` | #E8E8EA | Primary text |
| `--text-dim` | #9A9AA2 | Labels, axis ticks |
| `--accent` | #58A6FF | Focus, links, active nav |

### 2.2 Light ("concrete")

| Token | Hex | Use |
|---|---|---|
| `--bg` | #F6F6F7 | App background |
| `--bg-panel` | #FFFFFF | Panel surface |
| `--bg-raised` | #EFEFF1 | Hover rows, inputs, rail |
| `--line` | #E0E0E3 | Borders, gridlines |
| `--text` | #1B1B1E | Primary text |
| `--text-dim` | #5C5C66 | Labels, axis ticks |
| `--accent` | #0969DA | Focus, links, active nav |

### 2.3 Data series (functional accents — fixed order, per-theme via `var(--s1)`…)

| Index | Dark | Light |
|---|---|---|
| s1 | #58A6FF | #0969DA |
| s2 | #3FB950 | #1A7F37 |
| s3 | #D29922 | #9A6700 |
| s4 | #F85149 | #CF222E |
| s5 | #BC8CFF | #8250C8 |
| s6 | #39C5CF | #1B7C83 |

Status: ok s2, warn s3, crit s4 (per-theme values above).

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Display (big stats) | Figtree (SIL OFL) | 27px / 700, tabular-nums |
| Panel title | Figtree | 11px / 600, +0.05em, uppercase |
| Body, legend, table | Figtree | 12px / 400 |
| Axis ticks, readouts | IBM Plex Mono (SIL OFL) | 10px / 400, tabular |

## 4. Layout

- Sidebar (collapsible) 232px / 56px, 250ms width ease, toggle in header (`aria-expanded`); labels fade out collapsed; auto-collapse under 920px; active item gets a 2px left accent bar.
- Topbar 48px: context select · range · refresh · theme toggle.
- Body: KPI strip of 5 stat tiles → line panel (8 col) + horizontal bars (4) → scatter (6) + area (6) → status table (12).
- Grid 12 col, gutter 10px, panel padding 14px, radius 8px; 1px `--line` border; dark = no shadow, light = `0 1px 2px rgba(27,27,30,.08)`.

## 5. Charts (mandatory)

- Ticks: 1-2-5 nice steps from data min/max; 4–6 y, 5–8 x; y-domain `[niceFloor(min), niceCeil(max)]`, max never clipped.
- Both axes labeled; numbers SI-abbreviated; time `HH:MM`; mono font.
- Hover: crosshair + all-visible-series tooltip with nearest-x snap; tooltip flips near edges; scatter per-point tooltip; h-bar row highlight.
- Legend click toggles series (off = 35% swatch, axes recompute); legend hover dims others to 22%.
- Scatter points r=4, fill-opacity .8; warn/crit points get a 1.5px ring (shape redundancy, not color-only).

## 6. Motion

| Step | Spec |
|---|---|
| Page enter | Rail slides in 200ms; panels rise 14px + fade, stagger 50ms, 460ms cubic-bezier(.22,1,.36,1) |
| Ink-in | Panel SVGs start `grayscale(1) opacity(.7)` → full color over 350ms after line draw ends |
| Stat numbers | Count-up 650ms ease-out; delta badge pops 120ms after |
| Lines | stroke-dashoffset draw 750ms; scatter points pop r 0→4, stagger 8ms |
| H-bars | Scale-X from left, stagger 30ms |
| Hover | Tooltip/crosshair instant |
| Theme switch | 200ms surface crossfade; series swap via CSS vars |
| Reduced motion | No entrances/ink-in; final state rendered |

## 7. Don't

- No tinted grays — surfaces stay R=G=B
- No color on chrome; accents live in data, status, and focus only
- No shadows in dark theme; no radius above 8px
- Status never color-only (badges carry text; scatter outliers carry rings)
- Don't animate tooltips or crosshairs


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#161618; --bg-panel:#1D1D20; --bg-raised:#26262A; --line:#313136;
  --text:#E8E8EA; --text-dim:#9A9AA2; --accent:#58A6FF;
  --s1:#58A6FF; --s2:#3FB950; --s3:#D29922; --s4:#F85149; --s5:#BC8CFF; --s6:#39C5CF;
  --ok:#3FB950; --warn:#D29922; --crit:#F85149;
  --radius:8px; --space-unit:10px;
  --font-sans:"Figtree",system-ui,sans-serif; --font-mono:"IBM Plex Mono",monospace;
}
[data-theme="light"]{
  --bg:#F6F6F7; --bg-panel:#FFFFFF; --bg-raised:#EFEFF1; --line:#E0E0E3;
  --text:#1B1B1E; --text-dim:#5C5C66; --accent:#0969DA;
  --s1:#0969DA; --s2:#1A7F37; --s3:#9A6700; --s4:#CF222E; --s5:#8250C8; --s6:#1B7C83;
  --ok:#1A7F37; --warn:#9A6700; --crit:#CF222E;
}
```
