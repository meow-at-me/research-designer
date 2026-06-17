# vendor/

Optional self-hosted copies of the pinned libraries, for fully offline / CORS-free demos and
reproducible screenshots. Demos may load libs from a pinned CDN (default) **or** from here.

## Policy

- **Pin exact versions.** No `@latest`, no floating ranges. The version here must match the version
  recorded in `LICENSES.md`.
- **Add SRI to every CDN `<script>`/`<link>`.** Generate:
  ```sh
  curl -sL "<cdn-url>" | openssl dgst -sha384 -binary | openssl base64 -A
  ```
  then `integrity="sha384-…" crossorigin="anonymous"`.
- **Keep each library's LICENSE file** next to its vendored copy (e.g. `vendor/three/LICENSE`).
- **One motion lib per demo** (Motion *or* GSAP, not both). three.js may pair with either.
- Only libraries listed in `LICENSES.md` may be vendored. No Spline / proprietary runtimes.

## Suggested layout

```
vendor/
  motion/      motion.min.js   + LICENSE (MIT)
  gsap/        gsap.min.js, ScrollTrigger.min.js + LICENSE (GreenSock — note: free, not MIT)
  lenis/       lenis.min.js    + LICENSE (MIT)
  three/       three.module.js, addons/ + LICENSE (MIT)
  model-viewer/ model-viewer.min.js + LICENSE (Apache-2.0)
  ogl/         ogl.js          + LICENSE (MIT)
```
