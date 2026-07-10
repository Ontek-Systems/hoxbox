# Hero interaction system

This is the standard timing, sizing, and behavior model for the hero's
division-switching interaction (hover/focus/click on Furniture, Joinery,
Plants, Signage, Design & Build) and the button/tagline hover effects. Point
future work at this file and say "standardize the [logo/tagline/button/hero
animation]" to reapply it anywhere else on the site.

Implementation reference: [`src/components/Hero.astro`](../../src/components/Hero.astro).
Logo-specific sizing/scale rules live in [logo-system.md](./logo-system.md) —
this file covers timing and the tagline/button treatments.

## Division switch — timing model

Triggered by `setActive()` in `Hero.astro`'s inline `<script>`, on
`mouseenter`/`focus` of a division link (or `mouseleave` of the whole
division list, which resets to the first division).

All delays/durations are named constants at the top of the script:

| Constant | Value | What it controls |
|---|---|---|
| `logoDelayMs` | 200ms | Wait before the logo subline starts swapping |
| `copyDelayMs` | 200ms | Wait before the text block starts fading out |
| `copyExitMs` | 400ms | Text fade-out duration; also when the text content is actually swapped and href/data-label updated |

Sequence for one hover:

1. **t=0**: accent color (`--accent` custom property), background slide,
   and division-list indicator update immediately — no delay on these.
2. **t=200ms**: text block (`is-changing` class) starts fading out, and the
   logo subline starts its opacity/transform swap, in parallel.
3. **t=600ms** (200 + 400): text content is swapped (eyebrow/title/body/cta)
   and the `is-changing` class is removed, starting the fade-in.
4. **t=1200ms** (600 + 600): fade-in completes.

Fade-out and fade-in are deliberately different durations, defined as two
separate transitions so there's no dead pause between them:

- `.hero__copy.is-changing` (fade-out): `opacity`/`transform`, **400ms**.
- `.hero__copy` base state (fade-in, i.e. class removed): `opacity`/`transform`,
  **600ms**.

The same `is-changing` pattern also drives the **page-load entrance** — the
script adds `is-changing` synchronously then removes it two
`requestAnimationFrame`s later, so the hero copy fades in on mount instead of
just appearing.

The logo subline transition is **400ms** each way (matches `logoDelayMs` +
the CSS transition), with a 400ms `transition-delay` on the incoming image so
outgoing/incoming never visually overlap. See logo-system.md for the
per-division scale/offset values.

Cancellation: every timer is a module-level `let` cleared with
`window.clearTimeout` at the start of `setActive()`, so rapidly hovering
across divisions (or hovering off before a delay elapses) always cancels the
pending step cleanly — nothing queued from a previous hover can fire after a
newer one starts.

## Tagline (eyebrow) hover effect

- Font size: `calc(var(--text-xs) * 0.85)`, weight `700`.
- No size change and no pulse — the tagline text itself doesn't move.
- On hover, the underline switches to `currentColor` (i.e. whatever the
  tagline text color is): `.hero__eyebrow:hover .pencil-underline { background:
  currentColor; }`. Since the underline has no color of its own set, it
  inherits `color` from `.hero__eyebrow`, so this is generic — white tagline
  text → underline goes white on hover, black tagline text → underline goes
  black on hover, automatically, without hardcoding a color here. At rest the
  underline stays its assigned `--pencil-color` (currently `var(--accent)`,
  the active division's brand color).

## Button hover/press effect

Structure (see the `<a class="hero__button">` markup in `Hero.astro`):

```html
<a class="hero__button">
  <span class="hero__button-bg" aria-hidden="true"></span>
  <span class="hero__button-edge" aria-hidden="true"></span>
  <span class="hero__button-label" data-label="...">...</span>
</a>
```

- Font size: `var(--text-xs)` (i.e. 1×, same base unit as the tagline),
  weight `700`.
- `--btn-pad` is a single shared custom property (`0.75rem 1.5rem`) used by
  both the visible label and its `::after` overlay, so they always stay in
  sync — don't hardcode the padding twice if this changes.
- **Hover**: `.hero__button-bg` (the accent stripe, 3px at rest) widens to
  100%, becoming the full background — `width` transition, 350ms.
- **Text color crossing**: the label has a white `::after` clone
  (`content: attr(data-label)`) revealed via `clip-path: inset()`, using the
  *exact same box, duration (350ms), and easing* as the background sweep.
  Because both are percentages of the same element's width, the white text
  reveal tracks pixel-for-pixel with the color sweep — there's never a
  moment where the background is white and the text is also white. The
  `data-label` attribute is kept in sync with the visible text by
  `setActive()` whenever the CTA copy changes between divisions.
- **Edge fade-in**: a `.hero__button-edge` line — white, same 3px width as
  the original accent stripe — fades in on the left over the same 350ms as
  the accent sweep, starting at the same time, so it builds in gradually
  alongside the background rather than popping in abruptly once the sweep
  finishes. Use white here since the button background is light; on a
  dark-background button use black instead, or whatever reads as the
  "inverse" of the button's resting background.
- **Active/press**: label scales to `0.97` and the accent stripe darkens
  (`color-mix(in srgb, var(--accent) 80%, black)`), both fast (150ms/none —
  no separate transition needed on the darken since it's instant on `:active`).

## Reapplying this elsewhere

1. Copy the `.hero__copy`/`.hero__eyebrow`/`.hero__button`/`.hero__button-bg`/
   `.hero__button-label` CSS rules and the button's two-span markup verbatim.
2. Reuse the constants (`logoDelayMs`, `copyDelayMs`, `copyExitMs`, and the
   600ms fade-in) rather than re-deriving new timings — they're tuned to feel
   deliberate but not sluggish together as one system.
3. If a new page's tagline underline is a different color (e.g. black
   instead of the division accent), the pulse-to-color keyframe already
   handles it generically — just pass the right `color` prop to
   `PencilUnderline`.
