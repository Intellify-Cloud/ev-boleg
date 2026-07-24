# Frontend Architecture Assessment

## Current State

This Jekyll site uses `_assets` as the source asset tree, webpack for `assets/bundle.css` and `assets/bundle.js`, `_data/sitetext.yml` for most home page content, and small Liquid includes for sections.

High impact findings:

- Sass decisions were split between `site.scss`, Bootstrap defaults, and component files, so some Bootstrap overrides could not take effect reliably.
- `services.html`, `calculators.html`, and `calculators2.html` duplicated the same section and card markup.
- Several templates contained inline styles, invalid Liquid/HTML, and missing accessibility attributes.
- Generated `dist` Sass/CSS fragments were checked into `_assets` but not imported by the source bundle.

Medium impact findings:

- The project has a useful partial structure, but shared section patterns should be extracted only when duplication is real.
- Most homepage content is already data-driven. Keeping repeated section content in `_data` and repeated structure in includes is the right direction.
- Page spacing belonged in Sass rather than an inline conditional style block.

Low impact findings:

- Class naming mixes Bootstrap utilities and project classes. Future custom classes should stay component-oriented and avoid deep nesting.
- Image folders contain multiple apparent variants. Remove only after confirming production references.
- Dart Sass now replaces deprecated `node-sass`, but full `@use`/`@forward` migration should be handled separately from this low-risk refactor.

## Prioritized Improvements

High:

- Load project design tokens before Bootstrap variables.
- Reuse one icon-card section include for services and calculator sections.
- Remove unreferenced generated files from `_assets`.
- Keep the site buildable after source-level changes.

Medium:

- Move inline visual styles into Sass.
- Improve semantic landmarks and Liquid correctness.
- Add useful alt text, hidden decorative icons, and accessible labels.

Low:

- Continue splitting oversized partials when new duplication appears.
- Audit unused image variants and old root-level assets.
- Plan a dedicated Sass module migration from `@import` to `@use` and `@forward`.

## Architecture Decisions

- Preserved the existing Bootstrap 4 and webpack architecture.
- Kept Sass `@import` because Bootstrap 4 and the current source structure still rely on import ordering; modern Sass modules need a dedicated migration.
- Extracted repeated card-section markup but retained the existing include names so layout call sites remain stable.
