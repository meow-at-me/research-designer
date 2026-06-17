# prism-glass.md

> Theme: near-black editorial canvas that scrolls like a deck; frosted-glass panels float over a slow living aurora, lit on their edges. Sticky title columns, label chips, sections that rise in and drift away on scroll.
> Use case: process explainers, course slides, product education, premium portfolio sections.
> Icons: Lucide (ISC), stroke 1.5px. No emoji — line-style SVG only.
> Technique note: editorial-scroll structure + a properly-built glass material (not flat blur). Composed from multiple references; borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **2 — WebGL** (full-bleed aurora shader behind the glass) |
| Libraries | three.js (aurora) · Motion (pointer sheen + scroll parallax) · Lenis (smooth scroll) · native CSS scroll-driven (progress + section rise/drift) |
| The motion idea | A slow aurora drifts behind frosted glass; sections rise in and drift away on scroll; a light spot tracks the pointer across the hero glass; image cards parallax gently |
| The 3D idea | A fullscreen fragment-shader aurora (flowing fbm noise in the palette) — the *living backdrop* that makes the glass read as real glass; without it, frosted glass looks flat |
| Reduced-motion | Aurora frozen (one static frame); section reveals instant; no parallax, no sheen tracking, Lenis off |
| WebGL fallback | CSS multi-radial-gradient aurora (static) behind the same glass |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | Glass needs something to refract | Color lives in the aurora behind; UI chrome stays monochrome glass + white text |
| 2 | Depth through light, not borders | Surfaces separate via blur + saturate + edge light + inset highlights, not heavy outlines |
| 3 | Editorial scroll | Sticky title column; right masonry of glass cards; sections rise/drift on scroll |
| 4 | Restraint | One display size per section; emphasis by weight, not color |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--canvas` | #0C0D10 | Page (near-black, slight blue) |
| `--hi` | #F4F4F2 | Headlines, emphasis |
| `--mid` | #A6A6AE | Body copy |
| `--low` | #6E6E78 | Captions, meta |
| `--focus` | #CDE3EF | Focus ring only |
| `--glass-hi` | rgba(255,255,255,.14) | Glass fill top |
| `--glass-lo` | rgba(255,255,255,.03) | Glass fill bottom |
| `--edge` | rgba(255,255,255,.6) | Light-catching edge (top-left) |

Aurora palette (shader only, deliberately offset, never UI): indigo #2A2F6B · violet #6E47C9 · teal #1F8F86 · magenta #B4478F.

Rule: color appears only in the aurora. Buttons, chips, text stay glass/white. Emphasis = `--hi` + weight 600, never color.

## 3. Glass Recipe (the identity — apply exactly)

| Layer | Spec |
|---|---|
| Fill | `linear-gradient(150deg, var(--glass-hi), var(--glass-lo) 60%)` |
| Backdrop | `backdrop-filter: blur(18px) saturate(160%)` (and `-webkit-` prefix) |
| Light edge | 1px gradient border via masked `::before`: bright at top-left, fading to transparent |
| Inset highlights | `inset 0 1px 0 rgba(255,255,255,.40)` (top), `inset 0 -1px 0 rgba(255,255,255,.06)` (bottom) |
| Float shadow | `0 10px 40px rgba(0,0,0,.45)` |
| Frost grain | `::after` SVG `feTurbulence`, opacity ~.04, `mix-blend: overlay` |
| Specular sweep | diagonal white gradient that travels across on hover, .8s |
| Pointer sheen (hero) | radial highlight following the cursor (`@property --mx/--my`, animated by Motion) |

Blur scale: nav 12px · card 18px · hero lens 28px.

## 4. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| Display (H1) | Inter Tight | clamp(40px, 6vw, 72px) / 500 / -0.02em / 1.05 |
| Section head | Inter Tight | 28–32px / 500 |
| Body | Inter | 15–16px / 1.6 |
| Chip / meta | JetBrains Mono | 11–12px / 500, uppercase, +0.06em |

## 5. Layout

- Container 1240px, 24px gutter; section padding 14vh 0
- Topbar: fixed glass, gradient fade into canvas; 2px native progress bar at very top
- Hero: display headline over the aurora + a glass "lens" panel (intro + mono meta + buttons)
- Content sections: heading block (chip + H2 + body) above a **3-up row of uniform vertical glass cards** — image plate on top (180px), description below. Dark canvas throughout.
- People grid: same pattern — 3 vertical glass cards (Kitty Park, Ya-ong Kim, Calico Lee), avatar on top + name/role below
- CTA: centered glass band
- Mobile (<860px): left column stacks above; masonry → 1–2 cols; sheen/parallax off

## 6. Components

| Component | Spec |
|---|---|
| Label chip | glass, radius 999px, mono 11px uppercase, `--hi` text |
| Glass card (vertical) | glass recipe, radius 16px; image plate on top (180px, clipped, parallax), description below (chip + H3 + body, 22px pad); hover: specular sweep. Laid out 3-up, uniform height |
| Hero lens | glass, blur 28px, radius 22px, pointer-tracking sheen |
| Button (primary) | glass, brighter fill `rgba(255,255,255,.16)`, `--hi` text, radius 999px; hover edge brightens |
| Button (secondary) | glass, fill `--glass-lo`, `--mid` text |
| Divider | 1px fade `linear-gradient(90deg, transparent, rgba(255,255,255,.12), transparent)` |

## 7. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| Aurora | slow flowing fbm drift | 2 / three.js ShaderMaterial, time uniform |
| Progress bar | scaleX 0→1 | 0 / `animation-timeline: scroll(root)` + JS fallback |
| Section rise/drift | `.rv` opacity+Y: rise in on entry, drift up+out near top | 0 / `animation-timeline: view()`, range entry→exit (+ IO fallback) |
| Hero sheen | light spot follows pointer | 1 / Motion animating `@property --mx/--my` |
| Card image parallax | gentle translateY by scroll progress | 1 / Motion `scroll({target})` |
| Smooth scroll | inertial | 1 / Lenis (off when reduced) |
| Card hover | lift + specular sweep | CSS |
| Reduced motion | aurora static; reveals instant; no sheen/parallax/Lenis | — |

## 8. 3D / Shader

| Aspect | Spec |
|---|---|
| Scene | fullscreen `PlaneGeometry(2,2)` + `ShaderMaterial`, output clip-space directly |
| Shader | 5-octave fbm aurora, palette mixed by two noise fields, edge vignette to keep corners near-black |
| Performance | DPR cap ≤2; pause loop on `document.hidden`; static first frame on load |
| Fallback | CSS multi-radial-gradient aurora when WebGL unavailable |

## 9. Don't

- No opaque panels where glass is specified — translucency is the identity
- No accent color on UI — color lives only in the aurora behind the glass
- No body text directly on the aurora without a glass layer between
- No borders thicker than 1px; depth is blur + edge light
- No fast aurora; it drifts slowly or not at all
- No external HDRI/model/image files — shader and geometry are procedural

---

## CSS tokens

```css
:root{
  --canvas:#0C0D10; --hi:#F4F4F2; --mid:#A6A6AE; --low:#6E6E78; --focus:#CDE3EF;
  --glass-hi:rgba(255,255,255,.14); --glass-lo:rgba(255,255,255,.03); --edge:rgba(255,255,255,.6);
  --radius:16px; --radius-pill:999px;
  --font-display:"Inter Tight",sans-serif; --font-body:"Inter",system-ui,sans-serif; --font-mono:"JetBrains Mono",monospace;
}
@property --mx{ syntax:"<percentage>"; inherits:false; initial-value:50% }
@property --my{ syntax:"<percentage>"; inherits:false; initial-value:30% }
```
