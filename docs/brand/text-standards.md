# Text content standard

Source of truth for what "standardize the text content" means whenever it's asked for on
this site. Every eyebrow/title/subtitle-or-description group across sections and cards
must match these exactly — not just "use a token from the scale," but the literal
expression below. Copy it verbatim into new components; don't approximate.

Reference implementation: `.intro__*` in [Intro.astro](../../src/components/Intro.astro)
(the "About Us" section) is the canonical pattern. Everything else should match it.

## Eyebrow (small uppercase label above a title)

```css
font-size: calc(var(--text-xs) * 0.85);
font-weight: 700;
letter-spacing: 0.207em;
text-transform: uppercase;
color: var(--color-text);
margin: 0 0 var(--space-4); /* or margin-bottom: 1.6rem where the section predates this doc — bring in line when touched */
```

## Title / heading (h2/h3, the big display line)

```css
font-family: var(--font-display);
font-size: clamp(1.9rem, 3.2vw, var(--text-3xl));
text-transform: uppercase;
line-height: var(--leading-tight);
color: var(--color-text);
margin: 0 0 var(--space-3);
```

This is the one size — don't invent a bigger/smaller clamp per section or card. If a title
needs to fit on one line, add `white-space: nowrap` and/or tighten copy; don't scale the
font up to fill space.

## Subtitle / description (body copy under a title)

```css
font-size: calc(var(--text-lg) * 0.85);
color: var(--color-text-muted);
line-height: var(--leading-normal); /* when multi-line/wrapping; omit for a single short line */
margin: 0;
```

## Checklist when asked to "standardize the text content"

1. **Color** — headings/eyebrows use `var(--color-text)`. Body/description copy uses
   `var(--color-text-muted)`. Never a hardcoded hex or one-off opacity.
2. **Sizing** — copy the exact `font-size` expressions above verbatim. Don't pick a
   different token that's "close" (e.g. bare `--text-lg` instead of
   `calc(var(--text-lg) * 0.85)`) — it will visibly mismatch.
3. **Spacing** — use the `--space-*` scale for margins between eyebrow → title → subtitle,
   matching the values above. No arbitrary rem/px.
4. **Cross-check the nearest sibling**, not just this doc — if a closer analogous element
   exists in the same section (e.g. a card title inside a section that also has its own
   section title), both must resolve to the same computed font-size unless there's an
   explicit reason for a hierarchy difference.
