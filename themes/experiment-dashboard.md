# experiment-dashboard.md

> Theme: dense slate-dark experiment-tracking console — topbar + run-list sidebar + working canvas, mono tabular numerals, muted series colors, hand-rolled SVG charts that draw in once. A tool, not a landing page.
> Archetype: experiment-dashboard.
> Use case: ML experiment / evaluation dashboard — training curves, ablations, run comparison, metric cards, runs table.
> Base themes: theme-park.md `lab-console` (experiment-tracking app shell, runs table, sparklines, mono numerals, status dots) on a `nocturne`/`graphite` dark slate palette (low-glare night surfaces, frost accent). `graphite`'s ink-in reveal is an optional signature.
> Icons: Lucide (ISC), stroke 1.75px, 20×20. No emoji — line-style SVG only.
> Technique note: lab-console's research-console structure recolored for a low-glare dark board; all charts are inline SVG, no chart library. Borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **1 — Motion** (chart draw-in, count-ups, bar grow). **No 3D** — a dashboard's character is its data. |
| Libraries | Motion (count-up, line draw, bar grow, panel rise) · native CSS (hover crosshair, pulse) |
| The motion idea | On load panels rise + fade, lines draw left→right (`stroke-dashoffset`), KPI numbers count up (tabular mono prevents jitter), bars scale from baseline, the running-run status dot pulses. Hover crosshair/tooltip is always instant. |
| The 3D idea | None — Tier 2 excluded. |
| Reduced-motion | No draw-in, count-up, bar-grow, pulse, or panel rise; every panel renders in final state. Hover crosshair still instant. |
| WebGL fallback | N/A (no WebGL). Note explicitly. |
| "Live" data (static demo) | Hardcoded deterministic JS dataset; an optional `setInterval` appends points to the one *running* run — **paused under reduced-motion and `document.hidden`**; first paint is the static final frame; numbers swap instantly per tick (no per-tick animation). |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | App shell, not landing page | Persistent topbar + run sidebar + canvas; no hero, no marketing |
| 2 | Density is respect | 13px UI base, 36px rows, 8–10px gutters — information per viewport beats whitespace |
| 3 | Color identifies series | Chrome is slate/ink; saturated color appears only as run/series identity, status, focus |
| 4 | Numbers never jitter | Every metric, id, timestamp is mono + tabular; stats stay put while updating |
| 5 | Charts are instruments | Each panel: uppercase title, mono current value, ruled plot, ticked axes, toggle legend |

## 2. Color Tokens (dark default)

| Token | Value | Usage |
|---|---|---|
| `--bg` | #242933 | App background (slate, not pure black) |
| `--bg-panel` | #2E3440 | Panel / sidebar / table surface |
| `--bg-raised` | #3B4252 | Hover rows, inputs, selected |
| `--line` | #434C5E | Borders, gridlines |
| `--text` | #ECEFF4 | Primary text |
| `--text-dim` | #A7B1C2 | Labels, axis ticks, secondary |
| `--accent` | #88C0D0 | Focus, links, active tab, selection |

Series palette (run/line identity — assign in order, never on chrome): `#88C0D0` frost · `#A3BE8C` green · `#EBCB8B` yellow · `#D08770` orange · `#B48EAD` purple · `#BF616A` red.
Status: running `#88C0D0` (pulsing) · finished `#A3BE8C` · failed `#BF616A` · queued `#A7B1C2`.
Optional light variant via `[data-theme="light"]` (nocturne light tokens) — ship dark; toggle is a stretch.

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| UI | IBM Plex Sans | 13px / 1.45; section titles 15px / 600; page title 20px / 600 |
| Panel title | IBM Plex Sans | 11–12px / 600, +0.04em, uppercase |
| Metrics, ids, timestamps, axis ticks | IBM Plex Mono | 12–13px, `font-variant-numeric: tabular-nums` |
| Big stat (KPI) | IBM Plex Mono | 28px / 600, tabular |

Scale: 11 / 12 / 13 / 15 / 20 / 28 — nothing larger; this is a tool, not a poster.

## 4. Layout

- App frame: topbar 48px + left sidebar 236px (collapsible, auto under 920px) + fluid canvas, 12–16px padding.
- Sidebar: nav groups (Overview · Runs · Compare · Ablations · System) with 11px uppercase labels; **run list** rows 32px = 8px color dot + mono run id + status pill.
- Canvas: toolbar row (filter chips + range select + refresh) above a 12-col grid, 8–10px gutters, panel padding 12–14px, radius 6px, 1px `--line`, no shadow in dark.
- Panel flow: KPI strip (5–6 tiles) → training-curve line (8 col) + val-metric line (4) → method-comparison bars (6) + PSNR-vs-latency scatter (6) → runs table (12) + selected-run config (alongside or below).
- Footer: mandatory disclaimer + a persistent "Fictional data — demo" banner.

## 5. Components

| Component | Spec |
|---|---|
| KPI tile | 12px uppercase label + 28px mono value (count-up) + delta badge (±, colored ok/crit) + faint sparkline bg |
| Run-list row | 8px series dot + mono run id + status pill; selected = 2px `--accent` left inset |
| Status dot/pill | 8px circle + 12px label; running pulses (opacity 1→.4, 1.2s) |
| Metric panel | uppercase title + mono current value top-right + inline-SVG line chart; 1.5px series lines, optional 8% area fill |
| Line chart | inline SVG, viewBox; 1-2-5 nice y ticks (4–6) + x ticks (5–8, step/epoch); horizontal gridlines only; draw-in once |
| Bar chart | horizontal bars per method, value labels (mono), grow from baseline once |
| Scatter | PSNR (y) vs inference-time (x); r=4 points, fill-opacity .8; each baseline a labeled point; ours ringed |
| Crosshair tooltip | vertical hairline + card listing every visible series (swatch · name · value), nearest-x snap, flips near edge — **instant** |
| Legend | panel-bottom; click toggles series (off = 35% swatch, axes recompute); hover dims others |
| Runs table | row 36px; mono right-aligned cells; header 11px uppercase `--text-dim`; sticky first column; sortable; row hover `--bg-raised`; selected row left-inset accent |
| Config key-value | two-column: 12px `--text-dim` keys / mono values; 28px hairline rows |
| Tag | mono 11px, radius 3px, `--line` outline |

## 5b. Genre components — REQUIRED

| # | Required block | Notes |
|---|---|---|
| 1 | Topbar | project/run switcher, range select, refresh; "fictional data" banner visible somewhere |
| 2 | Run-list sidebar | ≥4 runs with color dot + mono id + status (≥1 running, ≥1 failed) |
| 3 | KPI strip | 5–6 metric tiles with deltas (best PSNR, latest LPIPS, SSIM, GPU-hours, throughput, active runs) |
| 4 | Training-curve panel | PSNR (or loss) vs step, ≥2 runs overlaid, legend toggles series |
| 5 | Validation-metric panel | LPIPS/SSIM over epochs |
| 6 | Method-comparison bars | WhiskerSplat vs ≥4 baselines |
| 7 | Tradeoff scatter | PSNR vs inference-time, each method a point |
| 8 | Runs table | run id / config / PSNR / SSIM / LPIPS / status / GPU-h, sortable, sticky first col |
| 9 | Selected-run config panel | key-value hyperparameters |
| 10 | Footer | mandatory disclaimer |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| Page enter | panels rise 14–16px + fade, stagger ~45ms | 1 / Motion (or 0 `view()`) |
| KPI numbers | count-up 650–700ms ease-out; delta badge pops after | 1 / Motion |
| Lines | `stroke-dashoffset` draw left→right 750–800ms, after panel lands | 1 / Motion |
| Bars | scale-Y/X from baseline, small stagger | 1 / Motion |
| (optional) ink-in | panel SVG grayscale→color 350ms after line draw | 1 / graphite signature |
| Running dot | opacity 1→.4 pulse, 1.2s infinite | 0 / CSS |
| Live tick | numbers swap instantly (no animation); loop paused on hidden/reduced | JS |
| Hover crosshair/tooltip | instant, always | 0 / JS |
| Reduced motion | all entrances/draws/count-ups/pulse off; final state | — |

## 7. 3D

Not applicable — Tier 2 excluded. A dashboard's character is its data, not WebGL.

## 8. Don't

- No hero/marketing sections, gradients, glows, or shadows (dark) — flat panels + hairlines only.
- No pie/donut charts; lines, bars, scatter only. No chart library — inline SVG.
- Series colors never on buttons, nav, or headings; never swap in saturated primaries.
- Don't center numbers — right-align, tabular mono, always.
- Don't animate hover affordances; crosshair/tooltip tracking is instant.
- No unlabeled "live" data without the fictional-data banner; all metrics invented.

---

## CSS tokens

```css
:root{
  --bg:#242933; --bg-panel:#2E3440; --bg-raised:#3B4252; --line:#434C5E;
  --text:#ECEFF4; --text-dim:#A7B1C2; --accent:#88C0D0;
  --s1:#88C0D0; --s2:#A3BE8C; --s3:#EBCB8B; --s4:#D08770; --s5:#B48EAD; --s6:#BF616A;
  --running:#88C0D0; --finished:#A3BE8C; --failed:#BF616A; --queued:#A7B1C2;
  --radius:6px;
  --font-sans:"IBM Plex Sans",system-ui,sans-serif; --font-mono:"IBM Plex Mono",monospace;
}
[data-theme="light"]{
  --bg:#ECEFF4; --bg-panel:#FFFFFF; --bg-raised:#E5E9F0; --line:#D8DEE9;
  --text:#2E3440; --text-dim:#5B677A; --accent:#3B7C8F;
}
```
