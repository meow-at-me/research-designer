# lab-console.md

> Theme: warm-paper application shell, compact data density, jewel-toned series colors, mono numerals — an experiment-tracking console.
> Use case: run dashboards, metric monitors, evaluation tables, internal research tools.
> Icons: Lucide (ISC), stroke 1.5px. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | App shell, not landing page | Persistent top bar + sidebar + working canvas; no hero, no marketing sections |
| 2 | Density is respect | 13px UI base, 36px rows, 8px gaps — information per viewport beats whitespace |
| 3 | Color identifies series | Chrome is paper/ink; saturated color appears only as run/series identity and status |
| 4 | Numbers are monospaced | Every metric, id, and timestamp uses the mono face with tabular figures |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | #FBF9F9 | App canvas (warm paper) |
| `--panel` | #FFFFFF | Cards, table, sidebar |
| `--ink` | #14141C | Primary text |
| `--ink-soft` | #6F7890 | Labels, secondary (gray-blue) |
| `--ink-faint` | #A6ACBE | Placeholders, disabled |
| `--border` | #E5E1DC | 1px panel borders, table rules |
| `--accent` | #4E79C4 | Primary actions, links, focus, selection |

Series palette (run/line identity — assign in order, never decorative):
`#4E79C4` blue · `#3E8E7E` teal · `#C9A23D` gold · `#9B6FB5` violet · `#C75D5D` red · `#6F7890` gray

Status: running `#4E79C4` (pulsing) · finished `#3E8E7E` · failed `#C75D5D` · queued `#A6ACBE`.

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| UI | IBM Plex Sans (SIL OFL) | 13px base / 1.45; section titles 15px / 600 |
| Page title | IBM Plex Sans | 20px / 600 |
| Metrics, ids, timestamps, axis | IBM Plex Mono (SIL OFL) | 12-13px, `font-variant-numeric: tabular-nums` |
| Big stat | IBM Plex Mono | 28px / 500 |

Scale: 11 / 12 / 13 / 15 / 20 / 28. Nothing larger — this is a tool, not a poster.

## 4. Layout

- App frame: top bar 48px (project switcher, search, actions) + left sidebar 240px + fluid canvas, 16px padding
- Sidebar: nav groups with 11px uppercase labels; run list rows 32px with 8px color dot + mono id
- Canvas: toolbar row (filter chips + view toggles, height 28px controls) above content
- Chart grid: repeat(auto-fill, minmax(320px, 1fr)), 8px gap; panels 220px tall
- Table region: full-width panel; horizontal scroll allowed, first column sticky
- All panels: `--panel` bg, 1px `--border`, radius 6px; no shadows at rest

## 5. Radius Scale (fixed — never freehand)

| Token | Value | Used on |
|---|---|---|
| base | 3px | Buttons, inputs, chips, tags |
| md | 6px | Panels, cards, table container |
| lg | 8px | Modals, popovers |
| full | 999px | Status pills, dots |

## 6. Components

| Component | Spec |
|---|---|
| Toolbar button | Height 28px, radius 3px, 1px `--border`, 12px/500; primary: `--accent` bg white text |
| Filter chip | Height 24px, radius 3px, `--bg` fill; active: `--accent` at 12% alpha + `--accent` text |
| Runs table | Row 36px; 12px mono cells; header 11px uppercase `--ink-soft`; row hover `--bg`; selected row: 2px `--accent` left inset |
| Status dot | 8px circle + 12px label; running dot pulses |
| Metric panel | 13px title + mono current value top-right + line chart; 1.5px series lines, no fill or 8% area fill |
| Chart axes | Hairline `--border`; 11px mono ticks; horizontal gridlines only |
| Sparkline | 90×20px inline in table cells, series color, no axes |
| Key-value config | Two-column list: 12px `--ink-soft` keys / 12px mono values; 28px rows, hairline separated |
| Tag | Mono 11px, radius 3px, `--border` outline |
| Tooltip / crosshair | Vertical hairline + 12px mono readout card, radius 3px, `--ink` bg white text |

## 7. Motion

| Target | Effect |
|---|---|
| Chart line draw | stroke-dashoffset reveal 0.4s ease-out, once |
| Live value | No animation on tick — numbers swap instantly (tabular-nums prevents jitter) |
| Running dot | opacity 1→0.4 pulse, 1.2s ease-in-out infinite |
| Hover (rows, buttons) | background-color 0.15s ease |
| Panel enter | opacity 0.2s; no translate |
| Reduced motion | pulse and draw-in disabled; everything static |

## 8. Don't

- No hero sections, gradients, glows, or shadows — flat panels and hairlines only
- No radius above 8px except pills; never round table cells
- Series colors never on buttons, nav, or headings
- No pie/donut charts; lines and bars only
- Don't center-align numbers — right-align in tables, always tabular mono
- No dark mode in this spec (pair with a dark engineering theme instead)


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#FBF9F9; --panel:#FFFFFF; --ink:#14141C; --ink-soft:#6F7890; --ink-faint:#A6ACBE;
  --border:#E5E1DC; --accent:#4E79C4;
  --s1:#4E79C4; --s2:#3E8E7E; --s3:#C9A23D; --s4:#9B6FB5; --s5:#C75D5D; --s6:#6F7890;
  --running:#4E79C4; --finished:#3E8E7E; --failed:#C75D5D; --queued:#A6ACBE;
  --radius-base:3px; --radius-md:6px; --radius-lg:8px; --radius-pill:999px;
  --font-sans:"IBM Plex Sans",system-ui,sans-serif; --font-mono:"IBM Plex Mono",monospace;
}
```
