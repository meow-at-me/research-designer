# <theme-name>.md

> Theme: <one-line visual character — palette, type, surface, and the motion/3D idea in one breath>.
> Archetype: <task name, e.g. research-project-page> | General/showcase.
> Use case: <where this theme belongs>.
> Base themes: <if a task archetype, the theme-park.md theme(s) its visual language derives from>.
> Icons: Lucide (ISC), stroke <1.5px>. No emoji — line-style SVG only.
> Technique note: composed from 2+ independent references; borrow techniques, never a single composition.

## 0. Motion & 3D Capability  ← REQUIRED, declare before anything else

| Field | Value |
|---|---|
| Highest tier | 0 native · 1 motion-lib · 2 WebGL (pick one — see GUIDELINES §5) |
| Libraries | e.g. "Motion + Lenis", "GSAP ScrollTrigger", "three.js", "none (Tier 0)" |
| The motion idea | one sentence: what moves, why it serves this theme |
| The 3D idea (if Tier 2) | what object/material/shader, and its static fallback |
| Reduced-motion behavior | what turns off / becomes opacity-only |
| WebGL fallback (if Tier 2) | the poster/CSS shown when WebGL is unavailable |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | | |
| 2 | | |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | | |
| `--ink` | | |
| `--accent` | | (brand-offset tokens: keep as specified, never shift toward the original) |

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| Display | | |
| Body | | |
| Mono/labels | | |

## 4. Layout

- Container / grid:
- Hero:
- Sections:
- Footer:

## 5. Components

| Component | Spec |
|---|---|
| | |

## 5b. Genre components — REQUIRED  ← task archetypes only (general/showcase themes delete this section)

The content blocks this page genre must contain. A demo missing any of these fails the spec.

| # | Required block | Notes |
|---|---|---|
| 1 | | |
| 2 | | |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| | | |
| Reduced motion | | — |

## 7. 3D (only if Tier 2)

| Aspect | Spec |
|---|---|
| Scene / object | |
| Material / shading | |
| Camera / interaction | |
| Performance | DPR cap, pause-when-hidden, draw-call budget |
| Fallback | static poster / CSS |

## 8. Don't

-
-

---

## CSS tokens

```css
:root{
  /* the same values as above, as custom properties — prose spec stays source of truth */
}
```
