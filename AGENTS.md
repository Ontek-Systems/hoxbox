## Execution speed

Move fast. Make the change and stop — don't screenshot, restart servers, curl-check,
re-read files back, or otherwise self-verify after routine edits (copy changes, font/style
swaps, small component tweaks). The user reviews the result themselves and will tell you
if something's wrong. Reserve verification (dev server + screenshot) for genuinely
non-trivial new features where you have no other way to know if it works — not as a
default habit after every edit. When in doubt, ship it and let the user react.

## Brand & copy reference

Before writing any marketing/site copy, case study text, or service descriptions, read
[docs/brand/copywriting.md](docs/brand/copywriting.md) (voice/tone rules and locked copy,
e.g. the Home strapline) and [docs/brand/company-profile.md](docs/brand/company-profile.md)
(full page-by-page transcription of the HOXBOX Company Profile PDF — the company's actual
terminology, phrasing, service names, and project facts/figures). Use these as the source
of truth instead of generic construction/interior-design copy, and check company-profile.md's
"Known inconsistencies" section before quoting any figures publicly.

Before designing or restructuring a whole section/page (not small tweaks — see "Execution
speed" above for what counts as small), read
[docs/brand/design-principles.md](docs/brand/design-principles.md). It's the distilled
client brief (palette, typography, layout, imagery rules) reconciled from the company
profile and the client's onboarding form. See
[docs/brand/onboarding-brief.md](docs/brand/onboarding-brief.md) for the raw source if you
need the client's exact original wording.

When building a *new* section or page component, also read
[docs/brand/section-build-standard.md](docs/brand/section-build-standard.md) — the
assembly-level playbook (section shells, header blocks, the static-split vs.
pinned-scroll-sequence layout choice, parallax mechanics, buttons, colour sourcing) that
keeps new components structurally identical to the rest of the site, not just on-brand.

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
