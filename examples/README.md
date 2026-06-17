# examples/

Self-contained interactive demos, one per spec (`<name>.html`), in two categories (flat folder, see
README): **task archetypes** (`research-project-page`, `experiment-dashboard`, `research-poster`,
`eval-interface`) and **general / showcase** (`liquid-chrome`, `prism-glass`). Unlike the static sibling
repo, these are **published as live pages** (GitHub Pages) — motion and 3D are the point, so they're
meant to be opened, not screenshotted.

Each demo must follow `GUIDELINES.md` §6: single file, pinned + SRI'd deps (or `vendor/`),
`prefers-reduced-motion` + WebGL fallback, static hero on load, footer disclaimer.

`shots/` holds the own-logo files used in demos (the only image-in-demo exception).
