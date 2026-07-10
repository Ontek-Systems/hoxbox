# Hox Box logo system

The logo is a fixed "HOX BOX" wordmark with a swappable division subline
underneath (Design & Build / Furniture / Joinery / Plants / Signage). This
is the standard for how it's built, calibrated, and applied anywhere in the
site. Point future work at this file and say "standardize the logo" to
reapply it.

## Source files

Original exports: [`HOX BOX LOGO/`](../../HOX%20BOX%20LOGO/) at project root
(spaced filenames, as delivered). Working copies used by the site live in
[`public/logo/`](../../public/logo/) with clean kebab-case names:

| File | Use |
|---|---|
| `hoxbox-main-white.svg` | Wordmark, dark backgrounds |
| `hoxbox-main-black.svg` | Wordmark, light backgrounds |
| `design-build-white.svg` | Design & Build subline, dark bg (black/white only, no brand color) |
| `design-build-black.svg` | Design & Build subline, light bg |
| `furniture.svg` | Furniture subline — pink `#ed4693` |
| `joinery.svg` | Joinery subline — yellow `#f6e31b` |
| `plants.svg` | Plants subline — green `#32b62b` |
| `signage.svg` | Signage subline — blue `#16ace4` |

All files share `viewBox width="315"` — this is what makes a single fixed
CSS width safe to apply uniformly across every file. Do not introduce a
subline file with a different viewBox width; it will break the shared
scale.

Furniture, Joinery, Plants, Signage each ship as a single flat brand-color
fill (no black/white variants) — that's intentional, they're divisions, not
neutral marks. Design & Build and Main are black/white only, since Design &
Build is the "core"/neutral division.

## Layout rule

- Wordmark is the fixed anchor. It never moves or resizes when the subline
  changes.
- Subline sits a fixed gap below it (`gap: 0.35rem` in the current
  implementation), grid-stacked (`grid-area: 1 / 1`) so every subline shares
  the exact same top edge.
- Subline width is fixed (currently `6.5rem` to match the wordmark's
  rendered width); height is `auto` per file, so taller sublines (Plants)
  extend further down without shifting anything above. This is deliberate —
  do not force a fixed height on the subline container.

## Known source-file quirks (measured, not eyeballed)

The batch SVG export cropped each file tight to its own ink, but not
symmetrically — some sublines' ink sits flush against the left edge with
slack left over on the right, so naive `width: 100%` + `justify-self:
center` isn't enough; the asymmetry is baked into the file's own geometry,
not a CSS bug.

Verified by parsing real path bounding boxes (script: install
`svgpathtools` via `pip3 install --user svgpathtools`, then load each SVG
with `svg2paths`, compare ink bbox center to `viewBox` center — see chat
history for the exact script if it needs re-running against new exports).

Measured horizontal ink offset from true center (of 315 viewBox units):

| File | Offset | Correction applied |
|---|---|---|
| Main (wordmark) | 0.000 | none — already centered |
| Furniture | 0.000 | none — already centered |
| Design & Build | −1.23 | `translateX(0.39%)` |
| Joinery | −1.04 | `translateX(0.33%)` |
| Plants | −0.64 | `translateX(0.2%)` |
| Signage | −1.09 | `translateX(0.35%)` |

If a subline SVG is ever re-exported, re-measure before assuming it's
centered — don't reuse these numbers blind.

## Calibrated per-division scale

Visual size still needed manual eyeball correction on top of the geometric
centering fix (source crops aren't perceptually equal-weight even once
centered). Current calibrated `--logo-scale` multipliers, confirmed correct
by the user:

| Division | Scale |
|---|---|
| Main / default | 1.0 |
| Design & Build | 1.02 |
| Furniture | 1.02 |
| Joinery | 1.02 |
| Plants | 1.02 |
| Signage | 1.02 |

(All currently converge on 1.02 except the wordmark itself, which stays at
1.0 — but keep these as independent per-file CSS custom properties, not a
single shared rule, since they were tuned one at a time and may need to
diverge again if any source file changes.)

## Transition behavior

Subline swap on hover is sequenced, not cross-faded, to avoid the old/new
logo overlapping mid-transition. See
[hero-interactions.md](./hero-interactions.md) for the full timing model
shared across logo, tagline, body copy, and button — the logo swap uses
the same 200ms hover pre-delay and 400ms transition as everything else.

Implementation reference: [`src/components/Hero.astro`](../../src/components/Hero.astro),
`.hero__logo-sub-img` rules, driven by the existing `setActive()` hover
handler (same function that swaps hero copy, background slide, and accent
color).

## Reapplying this elsewhere

To put the logo on a new page/component:

1. Copy the wordmark + relevant subline `<img>` markup pattern from
   `Hero.astro` (`.hero__logo`, `.hero__logo-word`, `.hero__logo-sub`,
   `.hero__logo-sub-img`).
2. Reuse the CSS rules verbatim, including the per-division `--logo-scale`
   and `--logo-shift-x` custom properties above — don't re-derive them.
2. Pick white or black wordmark/Design & Build variants based on background
   (light bg → black, dark bg → white). Furniture/Joinery/Plants/Signage
   don't have variants — they're always their brand color regardless of
   background.
3. Keep the fixed-width, auto-height, grid-stacked structure so the top
   stays anchored and shorter/taller sublines don't reflow anything above.
