# TOOLING.md

External tools surveyed for motion + 3D, with license verdicts and how-to-use.
**License is the top selection criterion** (this repo is public). Verdicts as of 2026-06.
✅ = fits this repo's values · ⚠️ = use with care · ❌ = excluded.

## Verdict table

| Tier | Tool | License | Verdict | Use for |
|---|---|---|---|---|
| 0 | CSS scroll-driven (`animation-timeline: scroll()/view()`) | native | ✅ | reveals, parallax, progress, reading indicators — runs on compositor thread, 60fps |
| 0 | View Transitions API | native | ✅ | section/page transitions, morphing |
| 0 | Web Animations API (WAAPI) | native | ✅ | JS-controlled timelines without a lib |
| 1 | **Motion** (motion.dev; ex-Framer Motion + Motion One) | **MIT** | ✅ **default** | `animate()` ~4kb, springs, stagger, scroll; vanilla API |
| 1 | **GSAP** (+ all plugins) | GreenSock "No Charge" (free, **not MIT**) | ✅ when needed | SplitText, ScrollTrigger, MorphSVG, DrawSVG |
| 1 | anime.js v4 | MIT | ⚠️ redundant | general engine — overlaps Motion; pick one |
| 1 | **Lenis** | **MIT** | ✅ | smooth/inertia scroll (~3kb), pairs with scroll-driven |
| 1 | Lottie / dotLottie | MIT | ⚠️ asset-gated | AE vector motion as JSON — only with self-made/CC0 files |
| 2 | **three.js** (vanilla) | **MIT** | ✅ **3D default** | real WebGL scenes, materials, custom geometry |
| 2 | **@google/model-viewer** | **Apache-2.0** | ✅ | `<model-viewer src="x.glb">` drop-in 3D, zero 3D code |
| 2 | **OGL** / **curtains.js** | **MIT** | ✅ | lightweight shader backgrounds (gradient/noise/ripple) |
| 2 | React Three Fiber + drei | MIT | ⚠️ needs build | declarative 3D in JSX — breaks single-HTML rule unless bundled |
| 2 | **Spline** | runtime MIT, **scenes = proprietary SaaS editor** | ❌ **excluded** | free-tier ToS / commercial+attribution concerns, editor lock-in, hosted-scene dependency — conflicts with self-made-assets + license-clean ethos |

## Why these defaults

- **Motion over GSAP as default**: MIT (cleanest for a public repo), tiny, WAAPI-based. Reach for GSAP
  only when a theme genuinely needs SplitText/MorphSVG/ScrollTrigger — and document it in `LICENSES.md`
  with the "not MIT, free" note.
- **three.js over R3F**: R3F is excellent but needs React + a build step, which breaks the
  single-self-contained-HTML rule. Vanilla three.js drops into one file via an import map.
- **model-viewer for "just show a 3D object"**: when a theme only needs to display/rotate a model,
  this is one tag and Apache-licensed — no hand-written WebGL.
- **OGL/curtains for shader fields**: far lighter than three.js when you only need a full-bleed
  animated shader background (the honest version of `gradient-canvas` / `grainy-blur`).

## How to use — pinned CDN patterns

> Always pin an **exact** version and add an `integrity` (SRI) hash. The versions below are shape
> examples — lock the exact patch you test against and generate its SRI before committing.
> Generate SRI: `curl -s <url> | openssl dgst -sha384 -binary | openssl base64 -A`

**Motion (default, Tier 1)** — ES module:
```html
<script type="module">
  import { animate, inView, scroll } from "https://cdn.jsdelivr.net/npm/motion@12/+esm";
  inView("[data-reveal]", (el) => animate(el, { opacity: [0, 1], y: [24, 0] }, { duration: 0.6 }));
</script>
```

**GSAP (only when SplitText/ScrollTrigger needed, Tier 1):**
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.13/dist/gsap.min.js" integrity="sha384-…" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.13/dist/ScrollTrigger.min.js" integrity="sha384-…" crossorigin="anonymous"></script>
```

**Lenis (smooth scroll):**
```html
<script src="https://cdn.jsdelivr.net/npm/lenis@1/dist/lenis.min.js" integrity="sha384-…" crossorigin="anonymous"></script>
```

**three.js (Tier 2)** — import map keeps it single-file:
```html
<script type="importmap">
{ "imports": {
  "three": "https://cdn.jsdelivr.net/npm/three@0.169/build/three.module.js",
  "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.169/examples/jsm/"
}}
</script>
<script type="module">
  import * as THREE from "three";
  import { OrbitControls } from "three/addons/controls/OrbitControls.js";
  // cap DPR, pause when document.hidden, static fallback if no WebGL — see GUIDELINES §6
</script>
```

**model-viewer (drop-in 3D):**
```html
<script type="module" src="https://cdn.jsdelivr.net/npm/@google/model-viewer@4/dist/model-viewer.min.js"></script>
<model-viewer src="assets/models/cat-tower.glb" camera-controls auto-rotate poster="assets/models/cat-tower.webp"></model-viewer>
```

**Native Tier 0** — scroll-driven with progressive enhancement:
```css
@supports (animation-timeline: view()) {
  [data-reveal] { animation: reveal linear both; animation-timeline: view(); animation-range: entry 0% cover 30%; }
  @keyframes reveal { from { opacity:0; transform: translateY(24px); } to { opacity:1; transform:none; } }
}
/* Fallback: IntersectionObserver toggles a class when @supports fails. */
@media (prefers-reduced-motion: reduce) { [data-reveal] { animation: none; opacity: 1; transform: none; } }
```

## Mandatory guards (see GUIDELINES §6)

- `prefers-reduced-motion: reduce` → motion off, opacity only, 3D auto-rotation/parallax off.
- WebGL feature-detect → static poster/CSS fallback; cap `devicePixelRatio ≤ 2`; pause render loop on
  `document.hidden` / offscreen.
- Hero renders statically on load (headless screenshot capture freezes compositor/virtual time).

## Sources

- GSAP now 100% free incl. all plugins (Webflow, Apr 2025): [Webflow blog](https://webflow.com/blog/gsap-becomes-free), [CSS-Tricks](https://css-tricks.com/gsap-is-now-completely-free-even-for-commercial-use/), [GitHub](https://github.com/greensock/gsap)
- Motion (ex-Framer Motion) independent + vanilla API: [motion.dev](https://motion.dev/magazine/framer-motion-is-now-independent-introducing-motion), [Motion One vs Framer Motion](https://motion.dev/blog/should-i-use-framer-motion-or-motion-one)
- CSS scroll-driven + View Transitions support 2026: [Josh Comeau](https://www.joshwcomeau.com/animation/scroll-driven-animations/), [DevToolbox guide](https://devtoolbox.dedyn.io/blog/css-scroll-animations-guide)
- three.js / R3F / drei (MIT): [react-three-fiber GitHub](https://github.com/pmndrs/react-three-fiber), [R3F vs three.js 2026](https://graffersid.com/react-three-fiber-vs-three-js/)
- model-viewer (Apache-2.0): [LICENSE](https://github.com/google/model-viewer/blob/master/LICENSE)
- Lenis (MIT) / Lottie / dotLottie (MIT): [Lenis GitHub](https://github.com/darkroomengineering/lenis), [dotlottie-web](https://github.com/LottieFiles/dotlottie-web)
- OGL / curtains.js (MIT, lightweight WebGL): [curtains.js GitHub](https://github.com/martinlaxenaire/curtainsjs/), [WebGL libs comparison](https://github.com/jsulpis/webgl-libs-comparison)
- anime.js v4 (MIT): [animejs.com](https://animejs.com/)
- Spline runtime (MIT) but proprietary editor / hosted scenes: [react-spline](https://github.com/splinetool/react-spline), [Spline docs](https://docs.spline.design/)
