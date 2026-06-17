# liquid-chrome.md

> Theme: near-black premium canvas, a single real chrome 3D object reflecting a procedural environment, ice-cyan accent used sparingly, oversized grotesque display with mono labels.
> Use case: product launches, rendering/graphics tools, premium hardware, studio landings.
> Icons: Lucide (ISC), stroke 1.5px. No emoji — line-style SVG only.
> Technique note: composed from multiple references; the 3D treatment is the point — borrow techniques, never a single composition.

## 0. Motion & 3D Capability

| Field | Value |
|---|---|
| Highest tier | **2 — WebGL** |
| Libraries | three.js (3D) · Motion (reveals) · Lenis (smooth scroll) · native CSS scroll-driven (progress bar) |
| The motion idea | A chrome form slowly rotates and tilts toward the pointer; below-fold content reveals on scroll with stagger; a hairline progress bar tracks scroll natively |
| The 3D idea | Procedurally generated metallic torus-knot, lit by a procedural `RoomEnvironment` (no external HDRI) for genuine moving reflections CSS cannot fake |
| Reduced-motion | Rotation + pointer parallax off (static first frame kept); reveals become instant; Lenis disabled |
| WebGL fallback | CSS radial-gradient chrome orb + grain; canvas hidden, no three.js init |

## 1. Principles

| # | Principle | Implementation |
|---|---|---|
| 1 | The 3D is the hero, not decoration | One real chrome object anchors the page; everything else is quiet around it |
| 2 | Reflection over color | Material reads as metal lit by environment; the page palette stays near-monochrome |
| 3 | One cool accent | A single offset ice-cyan for labels, links, progress — never fills |
| 4 | Quiet motion, real depth | Slow rotation + pointer tilt; reveals are restrained; nothing flashes |

## 2. Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | #0A0B0D | Page (near-black, not pure) |
| `--surface` | #121419 | Cards, bands |
| `--ink` | #F2F4F7 | Text |
| `--ink-soft` | rgba(242,244,247,.58) | Secondary text |
| `--line` | rgba(242,244,247,.12) | Hairline borders |
| `--accent` | #5BD8F0 | Labels, links, progress (deliberately offset ice-cyan — do not shift toward pure cyan) |

## 3. Typography

| Role | Font (SIL OFL) | Spec |
|---|---|---|
| Display | Space Grotesk | clamp(44px, 7vw, 104px) / 500 / -0.02em / line-height .98 |
| Body | Inter | 16–18px / 1.6 |
| Labels, stats | JetBrains Mono | 11–12px / 500, uppercase, 0.12em |

## 4. Layout

- Container 1280px, 24px side padding; section padding 120px
- Hero: 2-col — left text block (kicker, headline, sub, button pair, mono stat row), right 3D stage (canvas)
- Sections: principles trio (3 cards), spec band (oversized numerals), feature split, CTA band
- Footer: hairline top border, mono disclaimer

## 5. Components

| Component | Spec |
|---|---|
| Primary button | accent text on transparent, 1px accent border, radius 999px, 14px 28px; hover: accent fill, ink-dark text |
| Secondary button | `--ink-soft` text, 1px `--line` border, same radius; hover: border `--ink` |
| Card | `--surface`, 1px `--line`, radius 16px, 28px pad; hover: translateY(-4px), border `--accent` @ 40% |
| Mono label | 11px uppercase 0.12em `--accent`, with a 24px hairline lead rule |
| Progress bar | 2px fixed top, `--accent`, scaleX driven by native scroll-timeline |

## 6. Motion (detailed)

| Target | Effect | Tier / technique |
|---|---|---|
| Chrome object | rotation.y += slow; rotation x/z eased toward pointer | 2 / three.js rAF |
| Scroll progress bar | scaleX 0→1 across page | 0 / `animation-timeline: scroll(root)` + JS fallback |
| Card / row reveal | opacity 0→1 + translateY 28→0, 0.7s, stagger | 1 / Motion `inView` |
| Smooth scroll | inertial scroll | 1 / Lenis (off when reduced) |
| Reduced motion | rotation/parallax/Lenis off; reveals instant; static first frame | — |

## 7. 3D

| Aspect | Spec |
|---|---|
| Scene / object | `TorusKnotGeometry(1.05, 0.34, 300, 40, 2, 3)`, centered, slightly right of stage |
| Material / shading | `MeshStandardMaterial` metalness 1.0, roughness 0.14, envMapIntensity 1.15, light ink color |
| Environment | `RoomEnvironment` via `PMREMGenerator` (procedural — no external file) |
| Camera / interaction | Perspective 35°, z≈8.5 (frames the full knot with margin so rotation never clips); pointer tilt eased (lerp 0.05), no orbit drag |
| Performance | DPR cap ≤2; pause render loop on `document.hidden` and when stage offscreen; one static frame on load |
| Fallback | CSS radial-gradient orb + grain when WebGL unavailable |

## 8. Don't

- No second 3D object; one chrome form per page
- No color fills of `--accent`; it stays line/label/text only
- No fast spin, no bounce — rotation is slow and continuous
- No box-shadow "3D" — depth comes from the real render, not CSS lips
- No external HDRI / model files — environment and geometry are procedural

---

## CSS tokens

```css
:root{
  --bg:#0A0B0D; --surface:#121419; --ink:#F2F4F7; --ink-soft:rgba(242,244,247,.58);
  --line:rgba(242,244,247,.12); --accent:#5BD8F0;
  --radius:16px; --radius-pill:999px;
  --font-display:"Space Grotesk",sans-serif; --font-body:"Inter",system-ui,sans-serif; --font-mono:"JetBrains Mono",monospace;
}
```
