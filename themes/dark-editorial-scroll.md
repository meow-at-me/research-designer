# Design Spec — "Dark Editorial Scroll" UI

A full-bleed near-black page that scrolls like a slide deck: a sticky left text column, a right masonry grid of moody photo/text cards with label chips, and sections that rise in from below and drift away upward as you scroll.

---

## 1. Identity

| Item | Value |
|---|---|
| Codename | `dark-editorial-scroll` |
| Mood | Calm, editorial, gallery-like, premium |
| Use cases | Course slides, process explainers, portfolio sections, product education |
| Demo brand | CatTower LTD (virtual) |
| Demo people | Kitty Park, Ya-ong Kim, Calico Lee (virtual) |

## 2. Color Tokens

| Token | Hex | Usage |
|---|---|---|
| `--canvas` | `#0F0F0F` | Page background (full-bleed) |
| `--card` | `#1A1A1A` | Text cards, image card base |
| `--card-edge` | `#262626` | 1px card borders, dividers |
| `--chip-bg` | `#2A2A2A` | Label chips |
| `--text-hi` | `#F2F2F0` | Headlines, bold emphasis |
| `--text-mid` | `#9C9C98` | Body copy |
| `--text-low` | `#6B6B67` | Captions, meta |
| `--amber` | `#D97A2B` | Photo warmth accent (gradients only, never UI chrome) |
| `--focus` | `#CDE3EF` | Focus ring only |

Rules:
- Accent color appears only inside imagery, never on buttons or text.
- Emphasis inside body copy = `--text-hi` + weight 600, not color.

## 3. Typography

| Role | Face | Size / Line | Weight | Tracking |
|---|---|---|---|---|
| Display (H1) | "Inter Tight", sans | 56 / 1.05 | 500 | -0.02em |
| Body | "Inter", sans | 15 / 1.6 | 400 | 0 |
| Card body | "Inter", sans | 14 / 1.55 | 400 | 0 |
| Chip / meta | "Inter", sans | 11 / 1 | 500 | +0.04em |

Rules:
- One display size per section. No secondary headings inside cards.
- Emphasis spans (`<strong>`) max 2 per card.

## 4. Layout

Full-bleed near-black canvas (`--canvas` on `html, body`). No outer frame; sections flow continuously and replace each other on scroll. `--focus` (#CDE3EF) is used only for focus rings.

Grid per section: 12-col, max-width 1240px, gutter 24px, padding `18vh 56px 14vh`.

```
[fixed topbar: brand ......................... course label]
[2px scroll progress bar]

  [chip]                      [img]   [img-tall]   [img]
  H1 Display (sticky)         [text]  [        ]   [text]
  Body paragraph (36ch)       [img ]  [text    ]   [img ]
  cols 1-4                    cols 5-12: 3-col masonry

  --- next section scrolls up into the same positions ---
```

- Left column: span 4, `position: sticky; top: 16vh` -- title holds, then is pushed up by the next section.
- Right content: span 8. Variants: 2-col photo masonry (4.1) / 3-col masonry / stacked rows / 3-col people grid.
- Topbar: fixed, gradient fade into canvas; 2px progress bar at very top.
- Mobile (<760px): left column stacks above; masonry 2 cols, then 1 (<480px).

### 4.1 Masonry variant — photos + glass cards

The right column (span 8) as a **2-column masonry**: two vertical flex columns, 16px gap, `align-items:start`. Cells interleave moody **image cards** (5.2) and **glass description cards** (5.5), so photos and copy alternate down the page.

```
cols 1-4 (sticky)        cols 5-12: 2-col masonry
[chip]                   [ wide img  ]   [ tall img    ]
H1 Display               [glass card ]   [             ]
Body (36-40ch)           [ wide img  ]   [ glass card  ]
[link pills]
```

- One column leads with a **tall anchor image** (aspect `3:4.4`, the warm focal photo); the other leads with a **wide image** (`4:3`), then a glass card, then a wide image. Keep exactly one tall anchor per right column.
- Images fill their card (`object-fit:cover`) with the 5.2 treatment; no text is overlaid on photos — all copy lives in glass cards.
- Each cell is an `.rv` element; `--i` (0,1,2...) restarts per column so the stagger reads left-to-right then down.
- Collapse: < 600px the two columns stack into one and every image switches to `16:10` so nothing runs tall.

## 5. Components

### 5.1 Label chip
- `--chip-bg`, radius 4, padding 4px 10px, chip type style, color `--text-hi`.
- One chip per card / per section header. Acts as category metadata.

### 5.2 Image card
- Radius 10-12, overflow hidden, 1px `--card-edge` border, `--card` base behind the image.
- Aspect varies (3:4, 1:1, 3:5); masonry (4.1) uses tall anchor `3:4.4` + wide `4:3`.
- Image treatment: dark, warm, soft-focus (CSS: `saturate(0.9) brightness(0.82-0.85)` + subtle blur allowed).
- Optional chip pinned 12px from bottom-left. In the masonry variant, leave photos textless and carry copy in glass cards (5.5).

### 5.3 Text card
- `--card`, radius 10, padding 20px, 1px `--card-edge` border.
- Chip on top, then card body with at most 2 emphasis spans.

### 5.4 Floating panel (optional)
- White `#FAFAF8` card overlapping the right edge, radius 14, shadow `0 24px 60px rgb(0 0 0 / .45)`.
- Dark text, used for notes / transcript overlay.

### 5.5 Glass description card
- Frosted card that carries copy between photos in the masonry variant (4.1), or over imagery.
- `background: rgba(255,255,255,.06)`, `backdrop-filter: blur(14px) saturate(130%)`, `border: 1px solid rgba(255,255,255,.14)`, radius 12, padding 18px.
- Contents: one translucent **label chip** (`rgba(255,255,255,.15)` bg, white text) + body (14 / 1.55, `--text-hi`) with at most 2 `<strong>` emphasis spans.
- Accent color never appears here; emphasis is `--text-hi` + weight 600, per §2.
- Fallback: `@supports not ((backdrop-filter:blur(1px)))` → opaque `rgba(26,26,26,.92)` so the copy stays legible.

## 6. Iconography

- No emoji. Line-style SVG only (open-source set, e.g. Lucide / Feather style).
- Stroke 1.5px, `stroke="currentColor"`, `fill="none"`, 16–20px box.
- Icons inherit text color; never use accent color.

## 7. Motion

Easing: `--ease: cubic-bezier(.2,.7,.2,1)` for all transitions.

| State | Target | Effect | Duration |
|---|---|---|---|
| Enter (scroll down) | `.rv` cards | Rise 56px + fade in, staggered | 800ms |
| Enter (scroll down) | `.rv-soft` text | Rise 26px + fade in, staggered | 650ms |
| Exit (scroll past top) | all `.rv*` | Drift up 32px + fade out, no stagger | 450ms |
| Re-enter from above | section | Classes reset, entrance replays | -- |
| Card hover | Image | Scale 1.03 | 400ms |
| CTA hover | Button | Rise 2px | 300ms |

Stagger system:
- Each element gets `--i` (0, 1, 2, ...); enter delay = `var(--i) * 100ms`. Exit delay is always 0.
- Trigger: one `IntersectionObserver` per section (threshold 0.12, rootMargin `0 0 -10% 0`).
  - intersecting -> add `.in` (remove `.out`)
  - left past the top (`boundingClientRect.top < 0`) -> add `.out`
  - left below -> remove both, so the entrance replays on the next scroll down
- Scroll progress: fixed 2px bar, width updated in `requestAnimationFrame`.
- Reduced motion: all sections get `.in` immediately; no transitions, no smooth scroll.

## 8. Accessibility

- Body contrast: `--text-mid` on `--canvas` ≥ 4.5:1.
- Focus ring: 2px `#CDE3EF` outline, 2px offset.
- All images require `alt`; decorative gradients use `aria-hidden`.

## 9. Content Rules (dummy data)

- People: Kitty Park, Ya-ong Kim, Calico Lee only.
- Company: CatTower LTD only.
- No real publications, brands, or persons.
- Copy register: plain verbs, sentence case, no marketing filler.


---

## CSS tokens

Copy-paste starting point - the same values documented above, as CSS custom properties (the prose spec stays the source of truth).

```css
:root{
  --canvas:#0F0F0F; --card:#1A1A1A; --card-edge:#262626; --chip-bg:#2A2A2A;
  --text-hi:#F2F2F0; --text-mid:#9C9C98; --text-low:#6B6B67;
  --amber:#D97A2B; --focus:#CDE3EF;
  --font-display:"Inter Tight",sans-serif; --font-body:"Inter",sans-serif; --font-mono:"JetBrains Mono",monospace;
}
```
