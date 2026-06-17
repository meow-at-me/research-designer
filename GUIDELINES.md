# GUIDELINES.md

Standing rules for all work in this repository (theme specs, demos, docs).
Reference this file's path alongside any task prompt. Inherits the spirit of `theme-park.md`'s
guidelines and **extends them for motion and 3D**.

## 1. Language & Writing

| Rule | Detail |
|---|---|
| All `.md` files in English | Demo page *content* may be Korean when the theme targets it |
| Tables and short specs over prose | No decorative or descriptive filler |
| Self-contained themes | Each `themes/*.md` must work alone — no required cross-file references |

## 2. Icons — No Emoji

- No platform/mobile emoji anywhere (md, demos, UI copy). Sole exception: a struck-through list
  showing what is banned (README).
- Use line-style SVG: Lucide (ISC) or equivalent open set, inlined.
- Inline attrs: `fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"` (width may follow the theme, 1–2px). UI icons 16–24px.

## 3. Fictional Content Only

| Slot | Fixed value |
|---|---|
| People | Kitty Park (CEO / PI), Ya-ong Kim (CTO), Calico Lee (Head of Design) |
| Company | CatTower LTD |
| Sub-brands | Derive from the universe: CatTower Bank, Meowtrics, Whiskr Inc, Pawment Co, … |

**Research canon (for task-archetype demos).** Grows the *same* cat universe so research and business
demos cohere. Reuse the principals as senior authors; add juniors in the cat motif.

| Slot | Fixed value |
|---|---|
| Authors | Kitty Park, Ya-ong Kim, Calico Lee (senior) · Mittens Choi, Tabby Seo, Sphynx Han (junior) |
| Lab / affiliation | CatTower Vision Lab (CatTower LTD) · Whiskr Inc (industry collaborator) |
| Venue | Invent one with **no real acronym** — e.g. **PURRCV 2026** (Pan-Universal Rendering & Reconstruction in Computer Vision). Never CVPR / ICCV / NeurIPS / SIGGRAPH etc. |
| Dataset / benchmark | Invent, cat/CV-themed — e.g. **CT-Scenes** (CatTower Scenes), MeowNet-1M, PawPose |
| Method / baselines | Invent — e.g. method **WhiskerSplat**; baselines Meowtrics-NeRF, PawSplat, FelineGS |

No real people, companies, publications, banks, regulators, venues, **conferences, journals, datasets,
leaderboards, or author names**. **All metrics, scores, ablations, p-values, runtimes, and leaderboard
rows are invented and must be labeled fictional on the page** (e.g. a "fictional data" banner on
dashboards, the footer disclaimer everywhere).

## 4. License Rules (highest priority — this repo is public)

| Rule | Detail |
|---|---|
| No brand assets | No logos, proprietary fonts, site screenshots, copied imagery or copywriting |
| Fonts | SIL OFL / open only, by CDN, no font binaries committed. Set: Inter, Inter Tight, Geist, Geist Mono, Pretendard, Bricolage Grotesque, Archivo, Archivo Black, Lora, Instrument Serif, Noto Serif KR, JetBrains Mono, IBM Plex Sans, IBM Plex Mono, Space Grotesk, Figtree |
| Colors | Common hue families free; brand-offset tokens stay as specified, never shifted toward the original |
| No 1:1 cloning | Combine 2+ references; borrow techniques, not compositions |
| Naming | Name by visual character, never by brand ("x-like" prohibited) |
| **Third-party libraries** | **MIT / Apache / BSD / free-for-commercial only.** Every lib used in any demo must be listed in `LICENSES.md` with version + license + source URL. GSAP is allowed (free) but is *not* MIT — note that explicitly. **Spline and any proprietary-editor / hosted-scene runtime are excluded.** |
| **3D & motion assets** | `.glb`/`.gltf` models, `.lottie`/`.json` files, textures, HDRIs: **self-made or CC0 only**, committed under `assets/`. No marketplace or unclear-license assets. Treat exactly like the "self-made SVG only" image rule. |
| Own-logo exception | Logo slots use our own files in `assets/` (or `examples/shots/`): dark-bg logo on black, light-bg logo otherwise. Only image files allowed in demos. |

## 5. Motion & 3D — Tier System

Every theme declares, in its spec, the **highest tier** it uses. Reach up a tier only when motion/3D
is *core to the theme's character*, never as decoration.

| Tier | Means | Use for | Default reach |
|---|---|---|---|
| **0** | Native: CSS scroll-driven (`animation-timeline: scroll()/view()`), View Transitions API, WAAPI, CSS transitions | reveals, parallax, progress, section transitions | **start here always** |
| **1** | One motion lib via CDN: **Motion** (default, MIT, ~4kb) or **GSAP** (free, when SplitText / MorphSVG / ScrollTrigger is needed) | text splitting, complex timelines, scroll-pinned sequences | when Tier 0 can't express it |
| **2** | Real WebGL: **three.js** (vanilla, MIT) for custom scenes; `<model-viewer>` (Apache) for drop-in models; **OGL/curtains** (MIT) for shader backgrounds | genuine 3D objects, materials, shader fields | only when 3D *is* the theme |

Rules:
- **One motion lib per demo.** Don't load Motion and GSAP together. Three.js may pair with either.
- Pin exact versions + Subresource Integrity (SRI). See `vendor/README.md`.
- Prefer ES-module CDN + `<script type="importmap">` for three.js; keep the demo a single file.

## 6. Demo Page Requirements

- Single self-contained `.html`. External deps allowed only as **pinned, SRI'd CDN URLs** (fonts + the
  tier libs the theme declares), or self-hosted under `vendor/`.
- Implement the theme spec exactly, including its Don't section. One theme per page.
- **Task archetypes must include every component named in the spec's §5b "Genre components — REQUIRED."**
  A project page without authors/abstract/BibTeX, a dashboard without a comparison table, or an eval UI
  without rating/submit controls fails the spec.
- **Poster archetype:** target a fixed aspect that renders on screen *and* prints. Include `@page` +
  `@media print` and a print-safe static state (no motion in print).
- **`prefers-reduced-motion`: required for every animation.** Motion off → opacity-only; loops/floats static; 3D auto-rotation/parallax disabled.
- **WebGL/3D fallback: required.** Feature-detect; on unsupported or low-power/mobile, render a static CSS/SVG/poster fallback. Cap `devicePixelRatio` (≤2), pause render loop when offscreen / `document.hidden`.
- **Screenshot-readiness:** demo hero must render statically on load (no load-time entrance animation, no WebGL mid-flight) — headless capture freezes compositor/virtual time. Keep animated entrances below the fold.
- Footer disclaimer, always:

```
Demo page generated from themes/<theme>.md (research-designer).
All people, companies, products, and figures are fictional.
No affiliation with any real entity.
```

## 7. File Conventions

| Kind | Convention | Example |
|---|---|---|
| Theme spec | lowercase kebab | `themes/liquid-chrome.md` |
| Demo page | lowercase kebab, matches theme | `examples/liquid-chrome.html` |
| Meta docs | UPPERCASE at root | `README.md`, `GUIDELINES.md`, `TOOLING.md`, `LICENSES.md` |
| Template | `themes/_TEMPLATE.md` | — |
| Spec category | task-archetype vs general/showcase — both flat in `themes/`, distinguished only by README grouping (no subfolders) | `themes/research-poster.md` (archetype) · `themes/liquid-chrome.md` (general) |

Case is fixed once chosen — never rename by case only (git case-sensitivity conflicts).

## 8. Don't

- No new theme that duplicates the visual character of an existing one.
- No motion/3D as decoration — it must serve the theme's stated character.
- No un-pinned CDN URLs, no un-licensed libs, no unclear-license 3D/motion assets.
- No proprietary-editor runtimes (Spline etc.).
- No skipping the reduced-motion or WebGL fallback.
