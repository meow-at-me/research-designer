# Tokens Studio — Handoff Notes

Context for Claude Code. Summarizes decisions made about Tokens Studio and how
design tokens are used in this project. Read alongside `README.md`.

## TL;DR decision

- We adopt the **DTCG token format** and **Style Dictionary** build pipeline.
- We do **not** adopt the paid Tokens Studio Platform (SaaS).
- Tokens Studio itself is optional: any DTCG-compliant editor or hand-written JSON works.
- **Integration (2026-06-16):** added **additively**. The repo keeps its `themes/*.md` + single-file
  `examples/*.html` structure (no `designs/NNN/` migration) and its GUIDELINES font set. Tokens are the
  source of truth for shared values; demos inline a copy of `build/variables.css` so they still need no build.

## What Tokens Studio is

A design-system platform built around design tokens. Open-source at its core,
tool-agnostic (Figma, Penpot, GitHub, VS Code), and uses Style Dictionary for
multi-platform export. Products: a Figma plugin, a paid Studio Platform (SaaS),
and supporting libraries.

A design token is a single source-of-truth value (color, spacing, duration, etc.)
stored as JSON in the W3C Design Tokens (DTCG) format, shared between design and code.

## Licensing (what is safe to copy)

| Asset | License | Reuse |
|-------|---------|-------|
| DTCG token format / concept | W3C open standard | Free, no restriction |
| Tokens Studio Figma plugin code | MIT | Copy/modify/commercial; keep license notice |
| Tokenscript interpreter, Penpot sync | MPL 2.0 | Usable; modified MPL files must stay public |
| Studio Platform (SaaS) + Pro features | Proprietary, paid | Not copyable |
| Website copy, layout, branding | Copyright | Do not copy |

We only rely on the open standard (row 1) plus MIT-licensed tooling.

## Token conventions

Format: DTCG. Files use `$type`, `$value`, `$description`. Two layers:

| Layer | File | Direct use? | Contents |
|-------|------|-------------|----------|
| Primitive | `shared/tokens/primitive.tokens.json` | No | Raw color ramps, durations, easing curves, dimensions |
| Semantic | `shared/tokens/semantic.tokens.json` | Yes | Intent-named tokens aliasing primitives via `{reference}` |

### Motion model

Tokens store values, not behavior. Duration and easing are tokens; trigger logic
(scroll position, hover) is component code.

| Semantic token | Aliases | Use for |
|----------------|---------|---------|
| `motion.feedback` | `duration.fast` + `easing.out` | hover, focus, press |
| `motion.layout` | `duration.base` + `easing.in-out` | panels, toggles, reflow |
| `motion.entrance` | `duration.slow` + `easing.spring` | scroll reveals, mounts |

DTCG types in use: `color`, `dimension`, `duration`, `cubicBezier`, composite `transition`.

### Theme swapping

A theme overrides primitive values only. Semantic names stay constant, so one swap
re-tunes every motion/color at once. Demo ships three motion personalities:
Default / Calm / Lively (differ only in `duration.fast|base|slow`).

### Accessibility

`prefers-reduced-motion` is not a token. Components must collapse all durations to
`duration.instant` under reduced motion. Enforced in CSS, not tokens.

## Build pipeline

```
npm install
npm run build:tokens     # tokens -> build/variables.css (Style Dictionary, css/variables, outputReferences)
```

Config: `build-tokens.mjs` (programmatic Style Dictionary v4). Source = primitive + semantic token
files, plus `motion/{calm,lively}.tokens.json` for the `[data-motion]` personality blocks.
Demos inline a pre-generated copy of the CSS variables so they open without a build.

## Current repo state

This repo uses `themes/*.md` specs + single-file `examples/*.html` demos (not a `designs/NNN/` layout).
The token layer was added **additively** and the build was run & verified:

- `shared/tokens/` — `primitive.tokens.json`, `semantic.tokens.json`, `motion/{calm,lively}.tokens.json`, `README.md`.
- `build-tokens.mjs` + `package.json` — Style Dictionary build (`npm run build:tokens`).
- `build/variables.css` — generated; the landing and (incrementally) demos inline a copy.
- `index.html` (landing) proves the model: scroll reveal (`entrance`) + hover (`feedback`) tokens, a
  Default / Calm / Lively motion-personality toggle (primitive swap), and the reduced-motion fallback.
- The four task archetypes — `research-project-page`, `experiment-dashboard`, `research-poster`,
  `eval-interface` — remain as-is and can adopt the shared vars incrementally.

## Suggested next tasks

1. ~~Run `npm install && npm run build:tokens` and confirm `build/variables.css` matches the inlined values~~ — **done** (2026-06-16).
2. Migrate each archetype demo to inline `build/variables.css` and consume `--motion-*` / `--surface-*` vars (currently each demo keeps its own theme-local `:root`).
3. Add a color theme (light/dark) as another primitive override set, reusing semantic color names.
4. Extend the build with a JS/TS platform export for component code.

## Hard rules (do not break)

- All `.md` files in English. Spec files (`themes/*.md`) contain no demo content.
- Demo content uses fictional names/brands (Kitty Park, Ya-ong Kim, Calico Lee, CatTower LTD; research
  canon WhiskerSplat / PURRCV 2026 / CT-Scenes) and lives only in `examples/*.html`.
- Icons: Lucide SVG line icons, no emoji.
- Fonts: follow `GUIDELINES.md` §4 — the SIL OFL set (Inter, Inter Tight, Archivo, Lora, Pretendard,
  JetBrains Mono, IBM Plex, …). _(Supersedes the earlier "Google Sans Flex" note, which came from a
  different project context and is not used here.)_
