# LICENSES.md

Every third-party dependency used in any demo is listed here. Selection rule
(GUIDELINES §4): MIT / Apache / BSD / free-for-commercial only; proprietary-editor
runtimes excluded. Add a row the first time a library appears in a demo.

## This repository

| Part | License |
|---|---|
| Docs (`*.md`) | CC BY 4.0 |
| Code samples (demos, snippets) | MIT |

## Fonts (CDN, SIL OFL / open)

| Font | License | Source |
|---|---|---|
| Inter, Inter Tight | SIL OFL 1.1 | Google Fonts / jsDelivr |
| Geist, Geist Mono | SIL OFL 1.1 | jsDelivr |
| Pretendard | SIL OFL 1.1 | jsDelivr (orioncactus/pretendard) |
| Space Grotesk, Bricolage Grotesque, Archivo, Lora, Instrument Serif, Noto Serif KR, JetBrains Mono, IBM Plex Sans/Mono, Figtree | SIL OFL 1.1 | Google Fonts |

## JavaScript libraries

| Library | Version (pin) | License | Notes |
|---|---|---|---|
| Motion (motion.dev) | 12.40.0 | **MIT** | default motion lib (research-project-page, experiment-dashboard) |
| three.js | 0.184.0 | **MIT** | real 3D (research-project-page point-cloud viewer; liquid-chrome; prism-glass) |
| Lenis | 1.3.23 | **MIT** | smooth scroll (prism-glass) |
| GSAP + plugins | 3.13+ | **GreenSock "No Charge"** — free for commercial, **not MIT/OSI**. Used only in themes needing SplitText/ScrollTrigger/MorphSVG | flag the non-MIT status wherever bundled |
| @google/model-viewer | TBD | **Apache-2.0** | drop-in 3D models |
| OGL / curtains.js | TBD | **MIT** | shader backgrounds |
| Lottie / dotLottie | TBD | **MIT** | only with self-made/CC0 animation files |

## Build tooling & design tokens

| Item | Version (pin) | License | Notes |
|---|---|---|---|
| Style Dictionary | ^4.3.0 (devDependency) | **Apache-2.0** | builds `shared/tokens/*.tokens.json` → `build/variables.css` (`npm run build:tokens`); build-time only, not shipped in demos |
| W3C Design Tokens (DTCG) format | — | **open standard** | the `$type`/`$value` token JSON schema; no restriction |
| Tokens Studio Platform (SaaS) | — | proprietary | **not used** — any DTCG-compliant editor or hand-written JSON works |

## Icons & assets

| Asset | License | Notes |
|---|---|---|
| Lucide icons | ISC | inlined line-style SVG |
| 3D models (`.glb`), textures, HDRIs | **self-made or CC0 only** | committed under `assets/models/` |
| Lottie files (`.json`/`.lottie`) | **self-made or CC0 only** | committed under `assets/` |
| Own logo files | own asset | exception to "no external images" in demos |

## Excluded (do not use)

| Tool | Reason |
|---|---|
| Spline | scenes authored in proprietary SaaS editor; free-tier ToS / commercial+attribution concerns; hosted-scene dependency |
| Any unclear-license 3D model / marketplace asset | violates self-made/CC0 asset rule |
