# lumen.md

> Theme: deep-navy instrument panel — radial gauges, perceptually-uniform heatmaps, mono readouts, 8px baseline precision.
> Use case: lab consoles, instrument monitors, calibration/measurement UIs (dark default, light "paper" variant for printing).
> Icons: Lucide (ISC), stroke 1.5px, 20×20, round caps/joins. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Instrument, not app | Gauges, bands, colorbars, fixed-decimal mono readouts — measurement language |
| 2 | Precise and quiet | Everything aligned to an 8px baseline; medium-high density |
| 3 | Plot area is inset | Charts sit on `--bg-inset` one step below the panel so axes read clearly |
| 4 | Sequential color is principled | Heatmaps use a perceptually-uniform, colorblind-safe 7-stop ramp — same ramp in both themes |

## 2. Color Tokens

### 2.1 Dark (default)

| Token | Hex | Use |
|---|---|---|
| `--bg` | #0B1220 | App background |
| `--bg-panel` | #111A2C | Panels |
| `--bg-inset` | #0E1626 | Chart plot area |
| `--line` | #22304A | Borders, gridlines |
| `--text` | #E6EDF7 | Primary text |
| `--text-dim` | #8DA0BC | Labels, ticks |
| `--accent` | #35D0C5 | Active, focus, links |

### 2.2 Light ("paper")

| Token | Hex | Use |
|---|---|---|
| `--bg` | #F4F6F9 | App background |
| `--bg-panel` | #FFFFFF | Panels |
| `--bg-inset` | #F0F3F7 | Chart plot area |
| `--line` | #D9E0EA | Borders, gridlines |
| `--text` | #14233B | Primary text |
| `--text-dim` | #5C6F8A | Labels, ticks |
| `--accent` | #0E8F86 | Active, focus, links |

### 2.3 Data series (fixed order)

Dark: `#35D0C5` · `#7AA2F7` · `#E0AF68` · `#F7768E` · `#9ECE6A` · `#BB9AF7`
Light (darkened): `#0E8F86` · `#3D6AD6` · `#9A6B14` · `#C9405E` · `#5C8A2E` · `#7A4FC9`

### 2.4 Sequential ramp (heatmap, gauge arcs)

7 stops: `#440154 #443983 #31688E #21918C #35B779 #90D743 #FDE725` (unchanged across themes).
Gauge zones: ok = s1 teal, warn `#E0AF68`, crit `#F7768E`.

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Readouts (gauge value) | IBM Plex Mono (SIL OFL) | 26px / 600, tabular-nums |
| Panel title | Figtree (SIL OFL) | 12.5px / 600, +0.03em |
| Body | Figtree | 13px / 400 |
| Ticks, units | IBM Plex Mono | 10.5px / 400 |

## 4. Layout

- Sidebar (collapsible) 236px / 58px, 250ms ease, auto-collapse under 920px; logo + nav + context info + footer.
- Topbar: context select · id · range · export · theme.
- Body: row of 3 radial gauges + summary trio panel → signal lines with min–max band (8) + spectrum bars (4) → heatmap (8) + event log (4) → footer note.
- Panel radius 8px, 1px border; plot areas use `--bg-inset`.
- Gauge: 240° arc, thin track + value arc + 5 labeled major ticks.

## 5. Charts (mandatory)

- Ticks 1-2-5 nice steps; axis titles carry units (`Temperature (°C)`); y pads 5% of span, extremes never clipped; gauges show numeric major ticks across configured min–max.
- Hover: lines = crosshair + per-series dots + mono fixed-decimal tooltip; heatmap = cell outline in `--accent` + `row · time · value+unit` tooltip; gauges = min/avg/max of recent window.
- Legend under line charts: swatch + series + unit; click toggles; double-click isolates (solo).
- Min–max band: translucent band of rolling min–max under the primary line; toggleable from legend.
- Heatmap colorbar: vertical ramp with 5 labeled ticks mapped to the actual data range.

## 6. Motion

| Step | Spec |
|---|---|
| Page enter | Panels fade-rise 12px, stagger 40ms, 420ms cubic-bezier(.22,1,.36,1) |
| Gauges | Arc sweeps 0→value 850ms with 3% overshoot; readout count-up synced |
| Lines | Stroke draw 700ms; band fades in after |
| Heatmap | Cells fade in by column, 6ms stagger (left→right scan) |
| Log rows | Cascade 25ms apart |
| Theme switch | 180ms crossfade; sequential ramp unchanged |
| Reduced motion | Static render, gauges at final value |

## 7. Don't

- Readouts and ticks are always mono with tabular figures — never proportional digits
- The sequential ramp never changes per theme; categorical series do
- No shadows; depth comes from panel/inset steps and hairlines
- Never clip a measured extreme; pad the domain instead
- No decorative color — accent + series + ramp only


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#0B1220; --bg-panel:#111A2C; --bg-inset:#0E1626; --line:#22304A;
  --text:#E6EDF7; --text-dim:#8DA0BC; --accent:#35D0C5;
  --s1:#35D0C5; --s2:#7AA2F7; --s3:#E0AF68; --s4:#F7768E; --s5:#9ECE6A; --s6:#BB9AF7;
  /* sequential ramp (heatmap, gauge arcs) - unchanged across themes */
  --ramp1:#440154; --ramp2:#443983; --ramp3:#31688E; --ramp4:#21918C;
  --ramp5:#35B779; --ramp6:#90D743; --ramp7:#FDE725;
  --gauge-warn:#E0AF68; --gauge-crit:#F7768E;
  --radius:8px;
  --font-sans:"Figtree",system-ui,sans-serif; --font-mono:"IBM Plex Mono",monospace;
}
[data-theme="light"]{
  --bg:#F4F6F9; --bg-panel:#FFFFFF; --bg-inset:#F0F3F7; --line:#D9E0EA;
  --text:#14233B; --text-dim:#5C6F8A; --accent:#0E8F86;
  --s1:#0E8F86; --s2:#3D6AD6; --s3:#9A6B14; --s4:#C9405E; --s5:#5C8A2E; --s6:#7A4FC9;
}
```
