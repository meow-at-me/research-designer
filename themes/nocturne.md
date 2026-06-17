# nocturne.md

> Theme: dense night-ops observability board — slate-blue dark surfaces, frost accent, mono numerals, packed 12-col panels.
> Use case: telemetry/observability dashboards, on-call screens, NOC walls (dark default, light variant).
> Icons: Lucide (ISC), stroke 1.75px, 20×20, round caps/joins. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Low-glare night ops | Slate surfaces (not pure black), muted high-readability series hues |
| 2 | High density, calm rhythm | 12-col grid, 8px gutters, 12px panel padding — packed but ruled |
| 3 | Numbers never jitter | Every numeral is mono + tabular; stats stay put while updating |
| 4 | Panels are instruments | Each panel: uppercase title, ruled plot, ticked axes, bottom legend |

## 2. Color Tokens

### 2.1 Dark (default)

| Token | Hex | Use |
|---|---|---|
| `--bg` | #242933 | App background |
| `--bg-panel` | #2E3440 | Panel surface |
| `--bg-raised` | #3B4252 | Hover rows, inputs |
| `--line` | #434C5E | Borders, gridlines |
| `--text` | #ECEFF4 | Primary text |
| `--text-dim` | #A7B1C2 | Labels, ticks |
| `--accent` | #88C0D0 | Focus, links, active tab |

### 2.2 Light

| Token | Hex | Use |
|---|---|---|
| `--bg` | #ECEFF4 | App background |
| `--bg-panel` | #FFFFFF | Panel surface |
| `--bg-raised` | #E5E9F0 | Hover rows, inputs |
| `--line` | #D8DEE9 | Borders, gridlines |
| `--text` | #2E3440 | Primary text |
| `--text-dim` | #5B677A | Labels, ticks |
| `--accent` | #3B7C8F | Focus, links (darkened for contrast) |

### 2.3 Data series (fixed order)

`#88C0D0` frost · `#A3BE8C` green · `#EBCB8B` yellow · `#D08770` orange · `#B48EAD` purple · `#BF616A` red

Status: ok `#A3BE8C`/`#5E8A4E` · warn `#EBCB8B`/`#9A7B2D` · crit `#BF616A`/`#A8404A` (dark/light).

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Display (big stats) | JetBrains Mono (SIL OFL) | 28px / 700, tabular |
| Panel title | Figtree (SIL OFL) | 12px / 600, +0.04em, uppercase |
| Body, legend | Figtree | 12px / 400 |
| Axis ticks | JetBrains Mono | 10px / 400 |

## 4. Layout

- Sidebar (collapsible) 236px / 58px, 250ms ease, auto-collapse under 920px.
- Topbar 48px: breadcrumb · range · refresh · theme. Filter row 36px of selects beneath.
- Body: KPI strip of 6 stat tiles (sparkline backgrounds) → line (8) + bars (4) → line (6) + heatmap (6) → area (12).
- Grid 12 col, gutter 8px, panel padding 12px, radius 6px; 1px `--line` border; dark = no shadow, light = `0 1px 3px rgba(46,52,64,.10)`.

## 5. Charts (mandatory)

- Ticks 1-2-5 nice steps from min/max; 4–6 y, 5–8 x; y-domain `[niceFloor(min), niceCeil(max)]`, max never clipped.
- Every chart shows x ticks (`HH:MM`) and y ticks (SI-abbreviated `1.2K`, `3.4M`).
- Hover: vertical crosshair + tooltip listing every visible series (swatch · name · value), nearest-x snap, flips near right edge.
- Legend at panel bottom; click toggles series (off = 35% swatch); hover dims other series to 25%.
- Heatmap sequential ramp `#3B4252 → #88C0D0 → #EBCB8B → #BF616A`; cell hover shows `time / bucket / count`.

## Source provenance (hover → file:line) — REQUIRED

Every plotted datum is traceable to where it came from. Each chart point, bar, KPI value, and table row binds source metadata — the **file path and line number** of the log/record that produced it — and the hover tooltip / readout surfaces it as `path:line` beside the value.

| Aspect | Spec |
|---|---|
| Data model | each series/point carries `{ value, file, line }` (or a `src:"path:line"` string); a series maps point *i* → a deterministic line in that source log |
| Reveal | extend the theme's existing tooltip/readout — append `file:line` in mono, muted vs. the value; no new chrome |
| Example | hovering a point shows e.g. `4.2ms · logs/ops/controller.log:204` |
| Affordance | optional click-to-copy / link target; never required. Instant (no animation), like every hover here |
| Scope | a standing default for **every** data surface in this collection — not opt-in |

A number on screen must be traceable back to the exact line that produced it.

## 6. Motion

| Step | Spec |
|---|---|
| Page enter | Panels rise 16px + fade, stagger 45ms, 480ms cubic-bezier(.22,1,.36,1) |
| Stat numbers | Count-up 700ms ease-out (mono keeps width stable) |
| Lines | Draw left→right via stroke-dashoffset 800ms, after panel lands |
| Bars | Scale-Y from baseline, stagger 12ms |
| Hover | Crosshair/tooltip instant |
| Theme switch | 200ms crossfade |
| Reduced motion | All entrances off; values render final |

## 7. Don't

- No pure black, no pure white in dark theme — slate only
- Series hues stay muted; never swap in saturated primaries
- No radius above 6px; no shadows in dark
- KPI numerals never proportional — mono/tabular always
- Don't animate hover affordances; tracking is instant


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#242933; --bg-panel:#2E3440; --bg-raised:#3B4252; --line:#434C5E;
  --text:#ECEFF4; --text-dim:#A7B1C2; --accent:#88C0D0;
  --s1:#88C0D0; --s2:#A3BE8C; --s3:#EBCB8B; --s4:#D08770; --s5:#B48EAD; --s6:#BF616A;
  --ok:#A3BE8C; --warn:#EBCB8B; --crit:#BF616A;
  --radius:6px;
  --font-sans:"Figtree",system-ui,sans-serif; --font-mono:"JetBrains Mono",monospace;
}
[data-theme="light"]{
  --bg:#ECEFF4; --bg-panel:#FFFFFF; --bg-raised:#E5E9F0; --line:#D8DEE9;
  --text:#2E3440; --text-dim:#5B677A; --accent:#3B7C8F;
  --ok:#5E8A4E; --warn:#9A7B2D; --crit:#A8404A;
}
```
