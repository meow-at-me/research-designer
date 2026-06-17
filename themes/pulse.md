# pulse.md

> Theme: consumer-soft monitor — few large rounded cards, one blue accent, springy count-ups, "one glance and done".
> Use case: home/IoT monitors, personal stats, friendly status pages (follows system theme; toggle persists in-session).
> Icons: Lucide (ISC), stroke 1.8px, 22×22, round caps. No platform/mobile emoji — use SVG icons instead.

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | One glance and done | Low density: a hero number, a few large cards, generous whitespace |
| 2 | Motion is the product feel | Springy entrances, decimal-eased count-ups, silky theme switch |
| 3 | One blue, safe series | Single accent blue; data series from a colorblind-safe categorical set |
| 4 | Live, not static | Hero area chart draws beneath the number as a living background |

## 2. Color Tokens

### 2.1 Light

| Token | Hex | Use |
|---|---|---|
| `--bg` | #F2F4F6 | App background |
| `--bg-panel` | #FFFFFF | Cards |
| `--line` | #E5E8EB | Dividers, gridlines |
| `--text` | #191F28 | Primary text |
| `--text-dim` | #6B7684 | Secondary, ticks |
| `--accent` | #3182F6 | Primary blue |
| `--accent-soft` | #E8F3FF | Chips, hover wash |

### 2.2 Dark

| Token | Hex | Use |
|---|---|---|
| `--bg` | #101418 | App background |
| `--bg-panel` | #1B2026 | Cards |
| `--line` | #2A3138 | Dividers, gridlines |
| `--text` | #ECEFF1 | Primary text |
| `--text-dim` | #8B95A1 | Secondary, ticks |
| `--accent` | #4593FC | Primary blue |
| `--accent-soft` | #1B3A5C | Chips, hover wash |

### 2.3 Data series (colorblind-safe categorical — fixed order)

`#0072B2` blue · `#E69F00` orange · `#009E73` green · `#D55E00` vermilion · `#CC79A7` pink · `#56B4E9` sky

Status: good `#009E73`, watch `#E69F00`, alert `#D55E00`.

## 3. Typography

| Role | Font | Spec |
|---|---|---|
| Hero number | Figtree (SIL OFL) | 44px / 800, tabular-nums |
| Card title | Figtree | 15px / 700 |
| Body | Figtree | 14px / 400 |
| Ticks, units | Figtree | 11px / 500, tabular-nums |

## 4. Layout

- Max width 960px, centered; card radius 20px; gutters 16px.
- Sidebar (collapsible) 236px / 58px, 250ms ease, auto-collapse under 920px.
- Header: greeting · date · theme toggle.
- Body: hero card (big value + live area bg) → 3 stat cards (sparkline + delta) → multi-series line card (legend chips) → bars card → status-row list card.
- Shadows: light `0 2px 8px rgba(25,31,40,.06)`; dark none (1px `--line` border instead).

## 5. Charts (mandatory)

- Ticks: 1-2-5 nice steps; continuous quantities use padded data range (not zero-based); cumulative bars start at 0; 4–5 y ticks with units; x = `HH:MM`, 6 ticks.
- Hover: crosshair + snapping dot per series; tooltip card radius 12px at card elevation; touch scrubbing (tap-and-drag).
- Legend: pill chips above the chart showing live last value; tap toggles a series (chip turns outline).
- 24h chart annotates the daily min and max points with small labels.

## 6. Motion (the identity — apply exactly)

| Step | Spec |
|---|---|
| Page enter | Cards rise 24px + fade, spring cubic-bezier(.16,1.3,.3,1), 560ms, stagger 70ms top→bottom |
| Hero number | Count-up 900ms, decimal easing fast→slow; unit fades in after |
| Hero area | Path draws 900ms; gradient fill fades in over last 300ms |
| Delta chips | Slide-up 8px + fade at t=450ms |
| Bars | Spring-grow, 1.05 overshoot, stagger 14ms |
| List rows | Cascade fade 30ms apart |
| Theme switch | 220ms token crossfade; charts re-tint without redraw flash |
| Refresh | Button rotates 360°; numbers re-count |
| Reduced motion | Everything becomes a simple 150ms fade |

## 7. Components

| Component | Spec |
|---|---|
| Hero card | Radius 20px; oversized value + unit + live area chart as background layer |
| Stat card | Title + value + sparkline + delta chip (`--accent-soft` pill, arrow + text) |
| Legend chip | Pill button, `aria-pressed`; active = filled `--accent-soft`, off = outline |
| Status row | Icon + name + value + status dot-with-label; 44px min hit target |
| Theme toggle | Sun/moon swap, 220ms crossfade |

## 8. Don't

- Never more than one hero number per screen
- No dense tables, no 12-col packing — this theme is deliberately low-density
- No second accent hue; series colors never leak into chrome
- Status never color-alone (dot + text)
- No shadows in dark theme; no borders thicker than 1px


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --bg:#F2F4F6; --bg-panel:#FFFFFF; --line:#E5E8EB;
  --text:#191F28; --text-dim:#6B7684; --accent:#3182F6; --accent-soft:#E8F3FF;
  --s1:#0072B2; --s2:#E69F00; --s3:#009E73; --s4:#D55E00; --s5:#CC79A7; --s6:#56B4E9;
  --good:#009E73; --watch:#E69F00; --alert:#D55E00;
  --radius-card:20px; --space-unit:16px;
  --font-sans:"Figtree",system-ui,sans-serif;
}
[data-theme="dark"]{
  --bg:#101418; --bg-panel:#1B2026; --line:#2A3138;
  --text:#ECEFF1; --text-dim:#8B95A1; --accent:#4593FC; --accent-soft:#1B3A5C;
}
```
