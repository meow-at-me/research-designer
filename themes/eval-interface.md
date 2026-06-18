# eval-interface.md

> Theme: a clean slate-surfaced annotation **workbench** with one indigo selection accent; sticky header (progress · item counter · live session timer · pattern tabs) over a working canvas and a per-pattern bottom **dock**. Keyboard-first, low-distraction, with a dwell-guard before submit. Three judgment patterns in one shell.
> Archetype: eval-interface.
> Use case: human-verification / annotation & rating UI — **pairwise** A/B preference (+confidence), **Likert** multi-dimension rating, and **span** error labeling; progress, skip-with-reason, completion summary.
> Base themes: theme-park.md `dense-grid-shop` (keyboard focus discipline, snappy instant state) + `spec-minimal` (calm, low-distraction surfaces), rendered as a modern slate / indigo task shell.
> Icons: Lucide (ISC), stroke 2px — line-style SVG only. No emoji.
> Technique note: a worker-facing HIT shell that hosts multiple judgment patterns (pairwise / Likert / span); stimuli are text or pre-baked plates, not live 3D. Borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **0 — native** (vanilla JS + CSS). **No 3D**, no motion library — the tool's job is fast judgment, not spectacle. |
| Libraries | none — vanilla JS; CSS transitions (~150ms) only |
| The motion idea | Keyboard-first item flow: selecting fills instantly; a **dwell guard** (~3s countdown) before Submit unlocks discourages reflexive clicking; Submit / Skip advance the item (progress bar + counter step) with a toast; the live session timer ticks (pauses when the tab is hidden); a completion summary ends the batch. |
| The 3D idea | None — Tier 2 excluded. If stimuli are 3D reconstructions, render them as pre-baked static media plates. |
| Reduced-motion | All transitions/animations off; the dwell countdown collapses to 0 (instant unlock); the timer still ticks. |
| WebGL fallback | N/A (no WebGL). |
| Task flow (static demo) | A shared item counter (`Item N / total`) + progress bar; each **Submit** or **Skip** increments the item, resets the active pattern's inputs, and re-arms the dwell guard. Reaching `total` shows a **completion summary** (submitted / skipped / session time) with a restart. The three patterns share the shell via header tabs. |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | One judgment at a time | The current stimulus + its response controls own the viewport; a fixed bottom dock holds the verdict + Submit |
| 2 | Keyboard does everything | Number keys select, Enter submits, S skips, N marks "no errors", ? opens help; mouse is optional |
| 3 | Calm, not flashy | Slate surfaces, one indigo accent for the active/selected state; transitions ~150ms, never attention-grabbing |
| 4 | No accidental submits | Submit is disabled until a valid response exists **and** a short dwell guard elapses; skipping requires a reason |
| 5 | Multiple patterns, one shell | Pairwise / Likert / span share the header, dock, dwell guard, drawer, progress, and keyboard model |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--surface-page` | #F8FAFC | App background |
| `--surface-card` | #FFFFFF | Cards, header, dock |
| `--surface-sunken` | #F1F5F9 | Prompt card, inputs, key chips, tab track |
| `--border-default` | #E2E8F0 | 1px rules / card borders |
| `--border-strong` | #CBD5E1 | Control outlines (buttons, scale) |
| `--text-primary` | #0F172A | Headings, stimulus text |
| `--text-body` | #334155 | Body |
| `--text-muted` | #64748B | Labels, meta, counts |
| `--accent` | #4F46E5 | Selected choice, active scale/tab, progress, focus (indigo) |
| `--accent-soft` | #EEF2FF | Hover wash, selected-card tint |
| `--success` #16A34A · `--warning` #D97706 · `--danger` #DC2626 | — | Rated check · attention flag · errors/validation |

Span-label palette (semantic, pastel fills with darker text): factual error `#FECACA`/`#B91C1C` · unsupported claim `#FDE68A`/`#B45309` · contradiction `#BFDBFE`/`#1D4ED8`.
Focus ring (keyboard): `outline:2px solid var(--accent); outline-offset:2px`.
Radius: control 8px · card 12px · modal 16px · pill 9999px.

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| UI / body | IBM Plex Sans | 14px / 1.57; section labels 12px uppercase +0.06em; study/dim names 600 |
| Stimulus / prompt / response | IBM Plex Sans | 17px / 28px — generous reading size |
| Numerals, ids, timer, scale buttons, keys, span tags | JetBrains Mono | 12–14px, tabular |

## 4. Layout

- **Sticky header** (56px): study name · `Item N / total` (mono) · live timer · pattern **tabs** (Pairwise / Likert / Span) · Instructions. A 4px progress bar sits on top; an inline error **banner** drops below it on validation failure.
- **Canvas** (max 1200px): the active pattern pane.
- **Fixed bottom dock** (≥72px): a per-pattern row with the verdict controls, a Skip (with reason menu), and the primary Submit. The dock swaps with the tab.
- **Drawer** (right, 420px): instructions per pattern + payment/contact + fictional disclaimer.
- **Mobile (<768px):** desktop-only notice (annotation precision needs a pointer + width).

## 5. Components

| Component | Spec |
|---|---|
| Pattern tabs | pill segmented control in the header; switches both the canvas pane and the dock |
| Pairwise cards | prompt card + two response cards; selected card gets a 2px accent border + check; scrollable bodies |
| Confidence segment | appears after a choice; Low / Medium / High; required before submit |
| Likert dimension card | name + question + a 1–4 `radiogroup` with end anchors; rated cards get an accent left-rail + check; dock shows "n of N rated" |
| Span labels | armable chips (keys 1–3); arm one, then mouse-select text → wraps a `<mark>` with a superscript tag; a span list with per-row remove; "No errors" toggle |
| Dock buttons | Submit (primary, dwell-guarded, `Enter`), Skip (`S`, opens reason menu), per-pattern verdict controls with key chips |
| Dwell guard | Submit shows a `(3s…2s…1s)` countdown before it unlocks; collapses to instant under reduced-motion |
| Skip menu | small popover: "Content unclear / Cannot judge / Other" → records a skip + advances |
| Toast / banner | toast confirms record + next item; red banner for validation (e.g. missing confidence) |
| Completion summary | submitted / skipped / session-time tiles + restart |
| Drawer (instructions) | per-pattern how-to + payment/contact + fictional disclaimer |

## 5b. Genre components — REQUIRED

| # | Required block | Notes |
|---|---|---|
| 1 | Sticky header | study name, `Item N / total`, live session timer, pattern tabs, Instructions |
| 2 | Progress bar | advances on each submit/skip |
| 3 | Pairwise pattern | prompt + A/B responses + choose (A / Tie / B) + confidence |
| 4 | Likert pattern | source + summary-under-review + ≥3 dimensions, each a 1–4 scale with anchors |
| 5 | Span pattern | armable labels + mouse text-selection highlighting + span list + "no errors" |
| 6 | Per-pattern dock | primary Submit + Skip, keyboard-labelled |
| 7 | Dwell guard | Submit unlocks only after a short countdown |
| 8 | Skip-with-reason | menu of reasons, not a silent skip |
| 9 | Instructions drawer | per-pattern guidance + payment/contact + disclaimer |
| 10 | Completion summary | submitted / skipped / session time + restart |
| 11 | Keyboard model | 1–3 / Enter / S / N / ? |
| 12 | Footer disclaimer | mandatory fictional-content text (here: in the instructions drawer) |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| Selection / active state | instant accent fill | — / class toggle |
| Hover (buttons, scale, tabs) | ~150ms background / border ease | 0 / CSS transition |
| Submit dwell guard | countdown then unlock | 0 / JS timer (instant under reduced-motion) |
| Item advance | toast in/out, progress width step | 0 / CSS |
| Drawer / skip menu | slide / fade ~150ms | 0 / CSS transform |
| Reduced motion | all transitions + dwell collapse to 0; timer still ticks | — |

## 7. 3D

Not applicable — Tier 2 excluded. Stimuli are text or pre-baked plates so judgment stays fast.

## 8. Don't

- No motion library, no 3D, no decorative animation — this is a tool; transitions are functional and ~150ms.
- No accent beyond the active/selected state, progress, and focus; chrome stays slate/neutral. Span-label colors are semantic only.
- No submit without a valid response **and** the dwell guard; no silent skip (always capture a reason).
- No mixing two patterns in one verdict; the tabs are separate judgment modes.
- No real datasets/methods/metrics/workers — fictional CatTower canon only (Kitty Park, Ya-ong Kim, Calico Lee; study CT-2026-014).

---

## CSS tokens

```css
:root{
  --surface-page:#F8FAFC; --surface-card:#FFFFFF; --surface-sunken:#F1F5F9;
  --border-default:#E2E8F0; --border-strong:#CBD5E1;
  --text-primary:#0F172A; --text-body:#334155; --text-muted:#64748B; --text-inverse:#F8FAFC;
  --accent:#4F46E5; --accent-hover:#4338CA; --accent-soft:#EEF2FF;
  --success:#16A34A; --warning:#D97706; --danger:#DC2626; --danger-soft:#FEF2F2;
  --radius-control:8px; --radius-card:12px; --radius-modal:16px; --radius-pill:9999px;
  --font-sans:"IBM Plex Sans",system-ui,sans-serif; --font-mono:"JetBrains Mono",monospace;
}
```
