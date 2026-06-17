# shared/tokens

Design tokens for research-designer, in the **W3C Design Tokens (DTCG)** format
(`$type` / `$value` / `$description`), built to CSS custom properties with
**Style Dictionary**. This is an *additive* layer: the repo keeps its `themes/*.md` +
single-file `examples/*.html` structure and its no-build demos — see [`../../README.md`](../../README.md).

## Two layers

| Layer | File | Direct use? | Contents |
|---|---|---|---|
| Primitive | `primitive.tokens.json` | No | Raw color ramps, durations, easing curves, radii, spacing |
| Semantic | `semantic.tokens.json` | Yes | Intent-named tokens aliasing primitives via `{reference}` |

Components consume **semantic** tokens (`--text-hi`, `--motion-entrance-duration`, …);
primitives exist so a theme can re-tune everything by overriding a few raw values.

## Motion model

Tokens store values, not behavior. Duration and easing are tokens; trigger logic
(scroll position, hover) stays in component code.

| Semantic token | Aliases | Use for |
|---|---|---|
| `motion.feedback` | `duration.fast` + `easing.out` | hover, focus, press |
| `motion.layout` | `duration.base` + `easing.in-out` | panels, toggles, reflow |
| `motion.entrance` | `duration.slow` + `easing.spring` | scroll reveals, mounts |

Each is exposed as a pair of CSS vars so components compose them, e.g.
`transition: opacity var(--motion-entrance-duration) var(--motion-entrance-easing)`.

## Motion personalities (theme swap)

A personality overrides **primitive durations only**; semantic names stay constant, so one
swap re-tunes every motion at once. Set `data-motion` on `:root`:

| Personality | `data-motion` | fast / base / slow |
|---|---|---|
| Default | _(none)_ | 120 / 240 / 600 ms |
| Calm | `calm` | 200 / 420 / 900 ms |
| Lively | `lively` | 90 / 160 / 360 ms |

Defined in `motion/calm.tokens.json` and `motion/lively.tokens.json`. The landing page
(`/index.html`) wires a live Default/Calm/Lively toggle as a proof of the model.

## Accessibility

`prefers-reduced-motion` is **not** a token. Components collapse all durations to
`duration.instant` (0ms) under reduced motion — enforced in CSS, not tokens. Every demo
already does this in its `@media (prefers-reduced-motion: reduce)` block.

## Build

```
npm install
npm run build:tokens     # -> build/variables.css  (Style Dictionary, css/variables, outputReferences)
```

`build-tokens.mjs` emits one `build/variables.css` containing the default `:root` block
plus the two `[data-motion]` override blocks. **Demos inline a copy of these variables**
so they open with no build step — the JSON tokens are the source of truth; re-run the build
and re-paste when they change.

## Licensing

Only the open layer is used: the **DTCG format** (W3C open standard) and **Style Dictionary**
(Apache-2.0). The paid Tokens Studio Platform (SaaS) is **not** used; any DTCG-compliant
editor or hand-written JSON works. See [`../../LICENSES.md`](../../LICENSES.md).
