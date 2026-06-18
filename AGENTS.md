# Using research-designer

This repo is a library of self-contained **design specs**. To build a page:

1. Pick the one spec in `themes/` that matches what you're building (table below).
2. Follow it **exactly**, including its `Don't` section.
3. **One spec per page.** Never mix two — it defeats the purpose.
4. Output a single self-contained HTML file unless told otherwise.
5. Reach for motion (Tier 1) or 3D (Tier 2) only when the spec calls for it,
   never as decoration.

| Building...                          | Use                                              |
| ------------------------------------ | ------------------------------------------------ |
| Paper / project landing page         | `themes/research-project-page.md`                |
| Scroll-driven data story             | `themes/scrolly-data.md` / `dark-editorial-scroll.md` |
| Metrics / experiment dashboard       | `themes/experiment-dashboard.md` (+ aurora, graphite, lumen, nocturne, pulse, marine-grid, lab-console) |
| Human annotation / rating UI         | `themes/eval-interface.md`                       |
| Conference poster (screen + print)   | `themes/research-poster.md`                      |
| Pure motion/3D character study       | `themes/liquid-chrome.md`, `prism-glass.md`      |

See `GUIDELINES.md` for standing rules and `TOOLING.md` for the dependency catalog.
