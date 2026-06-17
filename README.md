# research-designer

**No time to design it. Too proud to ship it ugly.**

Motion- and 3D-first page specs **for researchers and engineers** — project pages, experiment
dashboards, eval / annotation interfaces, and posters **that aren't static**. A sibling to
**theme-park.md** (self-contained, license-clean specs that make generated pages stop looking generic),
but here **JavaScript, CSS, and real WebGL are first-class**: genuine motion and 3D, not box-shadow
"lips" and faked perspective.

> [!NOTE]
> Sibling project: `theme-park.md` focuses on HTML / shape / color in static
> Markdown specs. This repo extends that vocabulary into **kinetic and 3D** research surfaces.

## Who this is for

- **Researchers** shipping a paper / project page who want it *alive* — an interactive 3D
  reconstruction, a before/after comparison slider, scroll-driven figure reveals — not a static dump.
- **Engineers** building an internal **experiment dashboard** (training curves, ablations, run
  comparison) or a **human-verification / MTurk-style** data-collection interface.
- Anyone making a **conference poster** that renders on screen *and* prints to PDF in the same system.

The bundled demos all share one fictional computer-vision / 3D project (a feed-forward 3D
reconstruction method) so they read as a single body of work — fork a demo, swap in your own content,
keep the system.

## Task archetypes

The headline organizing concept: pick the archetype that matches what you're building. Each is a
self-contained spec **plus** a live demo.

| Archetype | What it is | Spec | Demo | Highest tier |
|---|---|---|---|---|
| **research-project-page** | Paper / project landing: title, authors, abstract, teaser, method, results, BibTeX | [`themes/research-project-page.md`](themes/research-project-page.md) | [`examples/research-project-page.html`](examples/research-project-page.html) | 2 (three.js) |
| **experiment-dashboard** | Metrics, ablations & run comparison — charts, KPI cards, readouts | [`themes/experiment-dashboard.md`](themes/experiment-dashboard.md) | [`examples/experiment-dashboard.html`](examples/experiment-dashboard.html) | 1 (Motion) |
| **research-poster** | Fixed-aspect poster (screen + print-to-PDF): columns, figure panels, QR | [`themes/research-poster.md`](themes/research-poster.md) | [`examples/research-poster.html`](examples/research-poster.html) | 0 (native, web only) |
| **eval-interface** | Human-verification / MTurk-style annotation & rating UI | [`themes/eval-interface.md`](themes/eval-interface.md) | [`examples/eval-interface.html`](examples/eval-interface.html) | 0 (View Transitions) |

## General / showcase themes

Pure motion/3D character studies that demonstrate the ethos in the abstract — reuse their techniques
inside any archetype.

| Theme | Character | Spec | Demo |
|---|---|---|---|
| **liquid-chrome** | Near-black premium; a real chrome torus-knot (three.js + RoomEnvironment) | [`themes/liquid-chrome.md`](themes/liquid-chrome.md) | [`examples/liquid-chrome.html`](examples/liquid-chrome.html) |
| **prism-glass** | Frosted glass over a living fbm-aurora shader (three.js) | [`themes/prism-glass.md`](themes/prism-glass.md) | [`examples/prism-glass.html`](examples/prism-glass.html) |

## What this is

Each spec is self-contained (`themes/*.md`) and pairs 1:1 with an interactive demo (`examples/*.html`).
Unlike the static sibling, **the live demo is the product** — motion and 3D can't be conveyed in a
screenshot — so demos are published (GitHub Pages) and meant to be opened, not just looked at.

## Approach: three-tier progressive enhancement

| Tier | Layer | Dependencies |
|---|---|---|
| 0 | Native CSS scroll-driven animations + View Transitions + WAAPI | none |
| 1 | Motion orchestration — **Motion** (default), **GSAP** when SplitText/ScrollTrigger is needed | CDN, pinned + SRI |
| 2 | Real 3D — **three.js** (custom scenes), `<model-viewer>` (drop-in), OGL/curtains (shader backgrounds) | CDN, pinned + SRI |

Every spec declares the highest tier it uses, and reaches for Tier 1/2 only when motion or 3D is core
to the task — never as decoration (a dashboard's character is its data, so it stays Tier 1; a poster
must print, so it stays Tier 0–1). See [`TOOLING.md`](TOOLING.md) for the tool catalog and license
verdicts, and [`GUIDELINES.md`](GUIDELINES.md) for the standing rules.

## Starter stack (all MIT / Apache / free-for-commercial)

```
Native first  →  CSS scroll-driven + View Transitions + WAAPI   (0 deps)
  + Lenis            (MIT)      smooth scroll
  + Motion           (MIT)      motion orchestration — default
  + GSAP             (free*)    SplitText / ScrollTrigger themes only   (*GreenSock license, not MIT)
  + three.js         (MIT)      real WebGL 3D
  + @google/model-viewer (Apache-2.0)  drop-in 3D models
  + OGL / curtains.js (MIT)     shader backgrounds
```

No proprietary editors, no hosted-scene lock-in (Spline is deliberately excluded — see TOOLING.md).
Every third-party dependency is listed with its license in [`LICENSES.md`](LICENSES.md).

## Repo layout

```
index.html     landing / gallery page (GitHub Pages root)
themes/        specs (.md), one per page — flat folder, two documented categories:
                 task archetypes:  research-project-page, experiment-dashboard, research-poster, eval-interface
                 general/showcase: liquid-chrome, prism-glass
               _TEMPLATE.md is the authoring template (one template covers both categories)
examples/      self-contained interactive demos (.html), 1:1 with themes/ — published as live pages
shared/tokens/ DTCG design tokens (primitive + semantic) — source of truth for shared values
build/         generated CSS variables (build/variables.css) — demos inline a copy
package.json   Style Dictionary build: `npm run build:tokens` (additive — demos still need no build)
vendor/        optional self-hosted copies of pinned libs (offline / CORS-free)
assets/        self-made or CC0 assets only — 3D models, lottie, previews, screenshots
GUIDELINES.md  standing rules (content + fictional-research universe, icons, fonts, motion/3D tiers, license manifest)
TOOLING.md     external-tool catalog + license verdicts + how-to-use
LICENSES.md    third-party dependency manifest
```

## Design tokens (optional layer)

Shared values (durations, easings, colors, radii, spacing) live as **DTCG design tokens** in
`shared/tokens/` and build to CSS custom properties with **Style Dictionary**
(`npm run build:tokens` → `build/variables.css`). This is **additive**: demos stay single
self-contained HTML and **inline a copy** of the variables, so nothing needs a build to open.
Motion is tokenised (`motion.feedback` / `layout` / `entrance` = duration + easing), and three
motion *personalities* — Default / Calm / Lively — swap only the duration primitives via
`:root[data-motion="…"]`. The landing page wires a live toggle as a proof. See
[`shared/tokens/README.md`](shared/tokens/README.md) and [`TOKENS_STUDIO_HANDOFF.md`](TOKENS_STUDIO_HANDOFF.md).

## Gallery (GitHub Pages)

Demos are published (not gitignored) — the deliberate difference from the static sibling. Each card links
to the live `examples/<name>.html`; open them, the motion and 3D are the point.

### Task archetypes

|  |  |
|---|---|
| [![research-project-page](assets/previews/research-project-page.png)](examples/research-project-page.html)<br>**research-project-page** — interactive 3D teaser, before/after slider, results, BibTeX | [![experiment-dashboard](assets/previews/experiment-dashboard.png)](examples/experiment-dashboard.html)<br>**experiment-dashboard** — training curves, run comparison, ablations, tradeoff scatter |
| [![research-poster](assets/previews/research-poster.png)](examples/research-poster.html)<br>**research-poster** — A0 portrait, renders on screen + prints to PDF | [![eval-interface](assets/previews/eval-interface.png)](examples/eval-interface.html)<br>**eval-interface** — keyboard-first A/B preference + Likert annotation |

### General / showcase

|  |  |
|---|---|
| [![liquid-chrome](assets/previews/liquid-chrome.png)](examples/liquid-chrome.html)<br>**liquid-chrome** — real chrome torus-knot (three.js) | [![prism-glass](assets/previews/prism-glass.png)](examples/prism-glass.html)<br>**prism-glass** — frosted glass over a living aurora shader |

**Status:** all four research archetypes built and verified. Published — live demos on GitHub Pages.

## License

Docs: CC BY 4.0 · Code samples: MIT · Third-party libraries: see [`LICENSES.md`](LICENSES.md).
Independent design-pattern study. Not affiliated with any company; no brand assets included;
all demo content (people, companies, datasets, venues, and metrics) is fictional.
