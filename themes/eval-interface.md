# eval-interface.md

> Theme: white, high-density, monochrome chrome with one warm-red selection accent; double keyboard focus ring, snappy instant state changes; full-bleed paired A/B media and uppercase micro-labels. A fast, low-distraction annotation tool.
> Archetype: eval-interface.
> Use case: human-verification / MTurk-style annotation & rating UI — A/B preference, Likert quality, attention checks, progress, submit.
> Base themes: theme-park.md `dense-grid-shop` (white monochrome chrome, warm-red active accent, double keyboard focus ring, snappy instant state) + `spec-minimal` (full-bleed paired media at a fixed ratio, uppercase wide-tracked wayfinding labels).
> Icons: Lucide (ISC), stroke 1.5px. No emoji — line-style SVG only.
> Technique note: a worker-facing HIT rendered in a dense, keyboard-first browse system; stimuli are pre-baked plates, not live 3D. Borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **0 — native** (View Transitions API + CSS), low distraction. **No 3D** (stimuli are pre-baked plates so the tool stays fast); no JS motion library. |
| Libraries | none — native View Transitions API (item-to-item cross-fade) · CSS (≤120ms confirm pulse) |
| The motion idea | Keyboard-first item flow: 1/2/3 selects with an **instant** accent fill (no transition — snappiness is the point), Enter advances, a fast View-Transition cross-fade swaps the outgoing/incoming item, progress bar steps. One ≤120ms CSS confirm pulse on the chosen choice is the only flourish. |
| The 3D idea | None — Tier 2 excluded. If stimuli are 3D reconstructions, render them as pre-baked static media plates. |
| Reduced-motion | View-Transition cross-fades disabled (instant swap), confirm-pulse off, progress bar updates without tween. Fully usable keyboard-only. |
| WebGL fallback | N/A (no WebGL). |
| Task queue (static demo) | Hardcoded JS array of ~12–20 items `{id, left, right, reference, prompt, gold?}`; state `{currentIndex, responses}`; re-render from state; "Submit" pushes a response + increments index; completion screen reads from `responses`. Optional `sessionStorage` resume (clearly a demo). First paint = item 1, static. |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Speed is the feature | State changes are instant; keyboard does everything; no animation between the worker and the next item |
| 2 | One judgment at a time | The current stimulus + its response controls own the viewport; chrome is minimal |
| 3 | Chrome stays monochrome | Nav, progress, legend in black/white/gray; warm red only marks the active/selected choice |
| 4 | No accidental submits | Submit is disabled until a response exists; attention checks and a "cannot judge" escape are first-class |
| 5 | Legible density | 12–13px base, tight but aligned; fixed media ratios so the layout never jumps between items |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | #FFFFFF | Page |
| `--panel` | #F7F7F7 | Instruction panel, input fills, media placeholders |
| `--ink` | #222222 | Text, active states, badges |
| `--ink-mid` | #717171 | Secondary text, counts, meta |
| `--border` | #DDDDDD | 1px rules, card/plate outlines |
| `--accent` | #E8474D | Selected choice, active radio, required marks (warm red, never shifted toward pink) |
| `--ok` | #1A7F37 | Confirmed / submitted / done dot |
| `--signal` | #FF7500 | Attention-check / warning marker (≤2 per view) |

Focus ring (keyboard, mandatory): double ring `0 0 0 2px #FFFFFF, 0 0 0 4px #222222`.

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| All text | Pretendard | Latin + Korean in one face (demo content may be Korean) |
| Study / section title | — | 18–20px / 700 |
| Prompt (the question) | — | 16px / 600 |
| Body / instructions | — | 13–14px / 1.5, `--ink-mid` |
| Micro-label / wayfinding | — | 11–12px / 600, uppercase, +0.12em tracking |
| Counts / progress / ids | — | 12–13px, `font-variant-numeric: tabular-nums` |

## 4. Layout

- Max 1100px centered app column; 16–24px padding; no marketing hero.
- **Top:** task header bar (study title left · requester + HIT-id + reward right) + a thin progress bar with "Item N / M".
- **Instruction panel:** collapsible, `--panel` fill, with a worked example; collapsed by default after first item.
- **Stimulus zone:** the reference plate on top (or left), then the **A | B pair** side-by-side at a fixed ratio (4:3), labelled `A`/`B` in uppercase micro-labels; a divider between. Single-stimulus Likert variant: one plate + the scale.
- **Response zone:** the 2AFC buttons (A better · Tie · B better) and/or the 1–5 quality scale, directly under the stimulus; comment + "cannot judge" below.
- **Footer of card:** keyboard legend + Prev / Skip / Submit-next.
- **Queue rail:** right (or bottom on narrow) — compact dots/rows for items, done/current/pending.
- Fixed media ratios so nothing reflows between items.

## 5. Components

| Component | Spec |
|---|---|
| Task header | study title + mono requester/HIT-id/reward; "Instructions" toggle (Lucide `info`) |
| Progress | 1px-track bar + `--accent` fill + mono "7 / 20"; steps instantly |
| Instruction panel | `--panel`, collapsible, worked example with a labelled correct answer |
| Reference plate | fixed-ratio media (self-made false-color/point-field render), `--panel` placeholder, `1px --border` |
| A/B pair | two equal fixed-ratio plates with uppercase `A`/`B` micro-labels; selected plate gets a 2px `--accent` outline |
| Choice button (2AFC) | rect, radius 4px, 1px `--border`; selected = `--accent` bg/border + white text, **instant**; keys 1/2/3 |
| Likert scale | 5 radio segments 1–5 with end labels (e.g. "poor"→"excellent"); selected = `--accent`; keys 1–5 |
| Comment field | optional textarea, `--panel` fill |
| Cannot-judge flag | checkbox + label; when set, allows submit without a preference |
| Attention check | occasional item with a known answer; `--signal` marker; mis-answer noted in summary |
| Keyboard legend | mono row: `1/2/3 choose · Enter submit · ← back · S skip` |
| Nav buttons | Prev (ghost) · Skip (ghost) · Submit-next (`--ink` bg, white; disabled until a response) |
| Queue rail | item rows/dots: done `--ok` · current `--ink` ring · pending `--border` |
| Completion screen | counts submitted/skipped, fictional inter-annotator agreement, WhiskerSplat win-rate |

## 5b. Genre components — REQUIRED

| # | Required block | Notes |
|---|---|---|
| 1 | Task header | study title, requester (CatTower LTD), HIT-id stand-in, reward |
| 2 | Progress indicator | "Item N / M" + bar |
| 3 | Instruction panel | collapsible, with a worked example |
| 4 | Stimulus | A/B reconstruction pair (+ reference); plus a single-stimulus Likert variant present |
| 5 | Response widget | 2AFC (A better / Tie / B better) **and** a 1–5 quality scale |
| 6 | Escape controls | optional comment + "cannot judge" flag |
| 7 | Keyboard-shortcut legend | 1/2/3, Enter, ←, Skip |
| 8 | Navigation + validation | Prev / Skip / Submit-next; no submit without a response (unless cannot-judge) |
| 9 | Queue rail | done / current / pending states across the item list |
| 10 | Completion screen | summary counts + fictional agreement / win-rate |
| 11 | Footer disclaimer | mandatory text |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| Item swap | fast cross-fade between outgoing/incoming item | 0 / View Transitions API (instant fallback) |
| Choice select | instant accent fill (no transition) | — / snappiness rule |
| Confirm | ≤120ms pulse on the selected choice | 0 / CSS animation |
| Progress bar | steps to new value, no tween needed | 0 / width set |
| Reduced motion | cross-fade off (instant swap), no pulse | — |

## 7. 3D

Not applicable — Tier 2 excluded. Stimuli are pre-baked plates so the tool stays fast and low-distraction.

## 8. Don't

- No animation between the worker and the next item beyond the fast cross-fade — selecting is instant.
- Color never on chrome; warm red is reserved for the selected/active choice and required marks.
- No submit enabled without a response (unless "cannot judge" is set); no silent advance past attention checks.
- No live WebGL stimuli; no shadows; no rounded corners beyond 4px; fixed media ratios — never reflow between items.
- No real datasets/methods/metrics — WhiskerSplat-vs-baselines study on CT-Scenes-Hard, all fictional.

---

## CSS tokens

```css
:root{
  --bg:#FFFFFF; --panel:#F7F7F7; --ink:#222222; --ink-mid:#717171; --border:#DDDDDD;
  --accent:#E8474D; --ok:#1A7F37; --signal:#FF7500;
  --radius-xs:2px; --radius-sm:4px; --radius-md:12px; --radius-pill:9999px;
  --ratio:4 / 3;
  --font-sans:"Pretendard",system-ui,sans-serif;
}
::view-transition-old(item),::view-transition-new(item){ animation-duration:.18s; }
```
