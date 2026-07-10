# Section / page build standard

Reference this whenever building a *new* section or page, to keep it structurally and
visually in line with what already exists — not just "on-brand colours" but the same
scaffolding, motion patterns, and building blocks. This doc is the assembly manual; the
docs it links out to are the individual parts.

Read alongside (don't duplicate their content, just apply it):
- [text-standards.md](./text-standards.md) — exact eyebrow/title/subtitle CSS. Every
  section built from this doc must also pass that checklist.
- [design-principles.md](./design-principles.md) — palette, typography, imagery sourcing
  rules, page-level requirements.
- [hero-interactions.md](./hero-interactions.md) — hover/timing model for interactive
  swaps (division switching, tagline/button hover).
- [logo-system.md](./logo-system.md) — division colour/logo sizing rules.
- [copywriting.md](./copywriting.md) — voice/tone, locked copy.

Canonical reference implementations: [Intro.astro](../../src/components/Intro.astro)
(static two-column section with parallax collage) and
[WhatWeDo.astro](../../src/components/WhatWeDo.astro) (pinned full-viewport scroll
sequence). Every pattern below is extracted from one of these two files — when in doubt,
go read the source rather than trust this doc's paraphrase.

## 1. Section shell

- Full-bleed `<section>`, no outer page gutter component — each section manages its own
  horizontal padding.
- Background is one of `--color-bg`, `--color-bg-light`, or `--color-graphite` (dark). Pick
  one flat background per section; no gradients on section backgrounds (gradients are only
  used for hairline dividers, see below).
- Vertical rhythm between sections: a 1px hairline divider, gradient from the section's own
  background colour up to ~60% graphite at the centre and back down — copy the
  `.what-we-do__divider` block verbatim and swap the two background colour stops.
- Standard vertical padding is `var(--section-pad-y)` top/bottom for content-driven
  sections. Full-viewport pinned sections (see §3) are the one exception — they size to
  `100vh`/`100svh` instead.
- Max content width `var(--content-width)`, centred with `margin: 0 auto` for text blocks;
  media can break out of that width but should stay inside the section's own padding.

## 2. Header block (eyebrow / title / subtitle)

Use `text-standards.md`'s CSS verbatim, and its structural pattern:

```astro
<p class="section__eyebrow">
  <span>LABEL</span><PencilUnderline color="var(--accent-or-division-color)" thickness={2} />
</p>
<h2 class="section__title">Title Case Or Sentence, Not All Caps In The Markup</h2>
<p class="section__subtitle">One or two sentences, muted colour.</p>
```

- `PencilUnderline` is the only underline treatment — never a plain `border-bottom` or
  `text-decoration`. It self-triggers on scroll via its own `IntersectionObserver`; don't
  re-implement that.
- Eyebrow copy is always short, uppercase in CSS (`text-transform: uppercase`), not typed
  in caps in the markup.
- Header block is either centred (`WhatWeDo`, standalone section intro) or left-aligned
  inside a copy column next to media (`Intro`, split layout) — pick based on whether the
  section is single-column or two-column, don't centre a two-column layout's header.

## 3. Choosing a layout: static split vs. pinned scroll sequence

Two established patterns — pick one, don't invent a third without a reason:

**A. Static two-column split** (`Intro.astro`) — copy on one side, image collage on the
other, normal document flow, `min-height: 100svh` but not pinned. Use this for content that
doesn't need a scroll-driven narrative (About-style sections, single features).

**B. Pinned full-viewport sequence** (`WhatWeDo.astro`) — `position: sticky` stage inside a
tall track (`track height = itemCount × 180vh` is the working ratio), items positioned
absolutely and translated in from off-screen (`--enter-x`/`--enter-y` custom properties),
promoted to `.is-active` as scroll progress crosses each item's bucket. Drives a progress
bar (`wwd-progress` pattern: track + fill + percentage label, all reusable verbatim). Use
this for a sequence of distinct items (services, categories, steps) that benefits from a
"reveal one at a time" narrative.

Both patterns must:
- Respect `prefers-reduced-motion: reduce` — snap straight to the settled/active state, no
  animation, motion script's `if` guard checks this before attaching scroll listeners.
- Collapse to a plain stacked mobile layout under the `860px` breakpoint: static grid
  columns become `1fr`, pinned tracks become `height: auto` normal flow, progress bar
  hidden.
- Use `requestAnimationFrame`-throttled scroll listeners (`ticking` boolean guard) — never
  an unthrottled `scroll` handler.

## 4. Parallax (optional, layerable onto either layout)

`Intro.astro`'s `data-parallax` system: any image wrapper gets `data-parallax
data-speed="0.85–1.15" data-dir="up|down|side"`. A single scroll listener computes one
`progress` value (–1 to 1, based on the section's centre relative to viewport centre) and
multiplies it by each element's speed/direction to set `--dx`/`--dy` custom properties,
consumed by `transform: translate3d(var(--dx,0px), var(--dy,0px), 0)`. To reuse:
- Keep speeds within roughly `0.7–1.3` — wider spreads look broken, not premium.
- Alternate `dir` between neighbouring elements so they shear apart rather than move as one
  block.
- One shared `progress` calculation per section, not one calculation per element.
- Gate behind the same `prefers-reduced-motion` check as §3.

## 5. Buttons

One button component pattern, two colour variants (light/`--accent` fill vs.
`--color-graphite` dark fill with accent edge) — copy `.intro__button*` /
`.wwd-card__button*` verbatim, don't restyle from scratch. Structure is always three
stacked spans: `-bg` (colour fill that wipes in on hover), `-edge` (2px accent line), `-label`
(the text, uppercase, `--text-xs`, `font-weight: 700`, `letter-spacing: 0.06em`).

## 6. Colour

- Division-specific accents pull from the five locked `--color-division-*` tokens — never
  hardcode a division's hex outside `tokens.css`.
- Section-level accent (when not division-specific) is set once via an inline
  `style="--accent:#…"` on the `<section>` and referenced as `var(--accent)` throughout —
  see `Intro.astro`'s `--accent:#d4302f`.
- Muted/body text is always `var(--color-text-muted)`, never a one-off opacity on
  `--color-text`.

## 7. New-component checklist

1. Section shell background + divider (§1).
2. Header block using exact text-standards.md CSS (§2).
3. Pick static-split or pinned-sequence layout, not a new pattern (§3).
4. Parallax only if the content benefits from it, same math as Intro (§4).
5. Buttons copied from the existing three-span pattern (§5).
6. Colour sourced from tokens only (§6).
7. `860px` mobile breakpoint collapsing to stacked flow, motion disabled.
8. `prefers-reduced-motion: reduce` guard on every scroll-driven script.
9. Cross-check the nearest sibling section for computed sizes before shipping — same rule
   as text-standards.md's checklist, applied to the whole section, not just text.
