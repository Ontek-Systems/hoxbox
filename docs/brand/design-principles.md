# Design principles — source of truth for visual direction

Distilled from [onboarding-brief.md](./onboarding-brief.md) (client's own words, 2026-07-08)
plus [company-profile.md](./company-profile.md). Read this before designing or restructuring
any section — not required for small tweaks (copy edits, animation timing, spacing nudges;
see CLAUDE.md's "Execution speed" rule for that line).

## The brief, in one line

Black-and-white, bold, minimal, luxury — division colors used sparingly as accents, not as
a rainbow. The opposite of the current live site, which the client wants stripped of
"oversized fonts."

## Palette

- Base: black `#000000` / white `#FFFFFF` only. This is the client's explicit answer, not
  a placeholder — confirm before introducing new base colors.
- Division colors (`--color-division-*` in [tokens.css](../../src/styles/tokens.css)) are
  for identifying divisions (Design & Build / Furniture / Joinery / Plants / Signage) via
  logo sublines and small accents — "to be used minimly" per the client. Don't tint large
  surfaces, backgrounds, or body text with them.
- Current `tokens.css` values (`--color-bg`, `--color-accent: #1f4d3f`, etc.) predate this
  brief and are marked as placeholders in the file itself — they need revisiting toward
  black/white once we're doing a deliberate palette pass, not fixed piecemeal.

## Typography

- "Bold" font style, "Luxury" vibe, "clean, stylish" — think confident and restrained, not
  loud. This is a correction against the current live site's "oversized fonts."
  Bold ≠ big — bold here reads as decisive weight/contrast on a clean layout, not oversized
  display type everywhere.
- `--font-display: "LEMONMILK"` (tokens.css) is the existing bold display face; keep using
  it for headings/logo-adjacent moments, but don't let every heading balloon in size to
  compensate — bold should come from weight and letterforms, not scale.
- Inspiration reference for tone: squarespace "klipsan-fluid-demo" — client's note: "clean,
  stylish, bold font."

## Layout / spatial feel

- "Simplicity, space" (client's note on the reseda-fluid-demo reference) — generous
  whitespace over dense composition. Prefer fewer elements per section, more breathing
  room, over packing content in.
- oktra.co.uk reference — "content very professional." Use this for tone of content
  density/structure (how much text per section, how corporate-clean the layout reads), not
  for color.

## Imagery

- Client has their own photography — "No, I have my own" on stock images, and "ill send
  all project photos via wetransfer" (separate folders per project). Do not source stock
  photography for hero/gallery imagery; wait for client-supplied assets. Brief calls for
  "cool edgy image[s]" on Home and About — so when the real photos land, favor the more
  striking/graphic shots over safe/generic ones when choosing crops or hero candidates.

## Confirmed page-level requirements

| Page | Requirement |
|---|---|
| Home | Animated logo (see [logo-system.md](./logo-system.md) + [hero-interactions.md](./hero-interactions.md) for the existing hover/transition system already built for this). Strapline: **"Integrity, reliability, and performance."** Edgy hero image (client-supplied, not stock). |
| About Us | Brief outline copy (short, not long-form) + another edgy image. |
| Services / Products | No brief given — use [company-profile.md](./company-profile.md) service list/descriptions as content source; layout is open. |
| Booking | No brief given. No booking system specified yet — flag with client before building any booking flow/integration. |
| Gallery | Portfolio of their projects — depends on client-supplied photos (WeTransfer, per-project folders). |
| Contact | No brief given. |
| Testimonials / Reviews | No brief given. |

## Open items to confirm with client before locking these sections in

- Primary logo choice: not provided (currently working from [logo-system.md](./logo-system.md)'s existing multi-division system — confirm this is still the intended logo, not a new mark).
- Booking system: unspecified — don't build/integrate one speculatively.
- Required features: unspecified.
- Social platform for `hoxboxdesignandbuild` handle: unspecified (Instagram vs Facebook vs both) — confirm before adding footer/header social links.
- Competitors: none listed, so no competitive-positioning cues available yet.
