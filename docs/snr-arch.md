You are a senior Frontend Architect specialising in Jekyll, Sass, BEM methodology, asset management and maintainable static-site architecture.

Your task is to clean up and restructure this Jekyll website without changing its current visual appearance or functionality.

The work should focus on:

* Converting the SCSS to consistent BEM notation
* Removing unused and orphaned files
* Cleaning out remnants from other websites or templates
* Reorganising images and video assets
* Centralising design variables and shared styling decisions
* Improving the overall maintainability of the codebase

Do not redesign the website.

Preserve the existing appearance, content and behaviour unless a small correction is required because of broken or duplicated code.

## 1. Convert SCSS to BEM Notation

Review all HTML, Liquid templates and SCSS files.

Convert component classes to consistent BEM notation:

```text
.block
.block__element
.block--modifier
.block__element--modifier
```

Examples:

```scss
.hero {}
.hero__content {}
.hero__title {}
.hero__description {}
.hero__actions {}
.hero__image {}
.hero--split {}
.hero--dark {}
```

```scss
.service-card {}
.service-card__icon {}
.service-card__title {}
.service-card__description {}
.service-card--featured {}
```

Apply BEM consistently across:

* Navigation
* Header
* Hero sections
* Buttons
* Cards
* Service sections
* Feature sections
* Testimonials
* Forms
* Calls to action
* Footer
* Modals
* Accordions
* Any other reusable components

Avoid vague class names such as:

```text
.left
.right
.blue
.box
.wrapper2
.content1
.section-new
.card-style
```

Use meaningful component-based names instead.

## 2. BEM Refactoring Rules

When converting the code:

* Update both the markup and SCSS together
* Preserve all functionality
* Preserve JavaScript selectors where required
* Prefer dedicated JavaScript hooks such as `.js-menu-toggle`
* Do not use visual style names as component names
* Keep nesting shallow
* Avoid selectors deeper than three levels
* Avoid styling based on element location
* Avoid relying on parent-page classes unless necessary
* Avoid IDs for styling
* Avoid `!important` unless there is no safe alternative

Prefer:

```scss
.feature-card {
  // Block styles

  &__icon {
    // Element styles
  }

  &__title {
    // Element styles
  }

  &--highlighted {
    // Modifier styles
  }
}
```

Avoid:

```scss
.home .features .cards .card div h3 {}
```

Do not create BEM elements that only exist to mirror every HTML tag.

Use BEM where it improves clarity and component ownership.

## 3. Separate Components From Utilities

Keep BEM classes for components.

Utilities should follow a separate naming convention, such as:

```text
.u-text-center
.u-hidden
.u-margin-top-lg
.u-container
```

JavaScript hooks should use:

```text
.js-menu-toggle
.js-accordion-trigger
```

State classes may use:

```text
.is-active
.is-open
.is-loading
.has-error
```

Do not mix state, utility and JavaScript behaviour into BEM component names unnecessarily.

## 4. Centralise Design Variables

Create one central design-token entry point for all shared visual values.

Recommended location:

```text
_sass/abstracts/_variables.scss
```

Alternatively, use:

```text
_sass/settings/_tokens.scss
```

Choose one naming convention and use it consistently.

Centralise:

* Brand colours
* Neutral colours
* Background colours
* Text colours
* Border colours
* Gradients
* Font families
* Font weights
* Font sizes
* Line heights
* Letter spacing
* Spacing scale
* Container widths
* Grid gaps
* Border radii
* Shadows
* Breakpoints
* Z-index values
* Transitions
* Animation durations
* Button heights
* Input heights

Example:

```scss
// Colours
$color-brand-primary: #000000;
$color-brand-secondary: #000000;
$color-text-primary: #000000;
$color-text-muted: #000000;
$color-background-page: #ffffff;
$color-background-surface: #ffffff;
$color-border-default: #dddddd;

// Typography
$font-family-primary: sans-serif;
$font-family-heading: sans-serif;

$font-weight-regular: 400;
$font-weight-medium: 500;
$font-weight-semibold: 600;
$font-weight-bold: 700;

// Spacing
$space-1: 0.25rem;
$space-2: 0.5rem;
$space-3: 0.75rem;
$space-4: 1rem;
$space-6: 1.5rem;
$space-8: 2rem;
$space-12: 3rem;
$space-16: 4rem;

// Radius
$radius-sm: 0.375rem;
$radius-md: 0.75rem;
$radius-lg: 1.25rem;
$radius-pill: 999px;

// Shadows
$shadow-sm: 0 2px 8px rgb(0 0 0 / 8%);
$shadow-md: 0 10px 30px rgb(0 0 0 / 12%);

// Layout
$container-sm: 48rem;
$container-md: 64rem;
$container-lg: 75rem;

// Breakpoints
$breakpoint-sm: 36rem;
$breakpoint-md: 48rem;
$breakpoint-lg: 64rem;
$breakpoint-xl: 80rem;
```

Do not leave repeated colours, spacing values, shadows, radii or breakpoints scattered throughout component files.

Avoid magic numbers where a shared token is appropriate.

## 5. Sass File Structure

Organise the Sass files into a clear structure.

Recommended structure:

```text
_sass/
  abstracts/
    _variables.scss
    _mixins.scss
    _functions.scss
    _breakpoints.scss

  base/
    _reset.scss
    _typography.scss
    _global.scss

  layout/
    _container.scss
    _header.scss
    _navigation.scss
    _footer.scss
    _grid.scss

  components/
    _button.scss
    _card.scss
    _hero.scss
    _form.scss
    _testimonial.scss
    _cta.scss
    _accordion.scss

  pages/
    _home.scss
    _about.scss
    _contact.scss

  utilities/
    _spacing.scss
    _display.scss
    _text.scss
```

Use modern Sass modules with:

```scss
@use
@forward
```

Do not continue using deprecated `@import` unless required by the current Jekyll or Sass environment.

If the installed Jekyll Sass processor does not support `@use`, confirm that before restructuring imports.

## 6. Clean Up Fonts

Audit all font files in the project.

Look for:

```text
.eot
.woff
.woff2
.ttf
.otf
.svg font files
```

Identify:

* Fonts that are not referenced anywhere
* Duplicate font families
* Duplicate font weights
* Old icon fonts
* Fonts inherited from previous themes
* Fonts related to other websites
* Fonts that exist only in unused CSS
* Legacy `.eot` files that are no longer required

Before deleting any font file:

1. Search all HTML, Liquid, Sass, CSS, JavaScript and configuration files.
2. Check all `@font-face` declarations.
3. Check generated stylesheets where relevant.
4. Confirm that no active component references the file.
5. Confirm that the file is not required by an icon system.

Remove confirmed orphaned files.

Prefer modern web font formats:

```text
.woff2
.woff
```

Keep `.ttf` or `.otf` only where there is a confirmed requirement.

Remove `.eot` files unless legacy Internet Explorer support is explicitly required.

Consolidate active font declarations into one file:

```text
_sass/base/_fonts.scss
```

Example:

```scss
@font-face {
  font-family: "Example Sans";
  src:
    url("/assets/fonts/example-sans-regular.woff2") format("woff2"),
    url("/assets/fonts/example-sans-regular.woff") format("woff");
  font-style: normal;
  font-weight: 400;
  font-display: swap;
}
```

## 7. Remove Remnants From Other Websites

Audit the project for files and code that appear to belong to:

* Other client websites
* Previous Jekyll themes
* Demo content
* Starter templates
* Old brand identities
* Old logos
* Unused colour systems
* Unused page layouts
* Old navigation variants
* Previous hero sections
* Unused JavaScript libraries
* Duplicate components
* Old screenshots
* Placeholder images
* Sample videos
* Backup files
* Temporary exports
* Test pages

Search for suspicious:

```text
client names
old domains
old company names
template branding
demo text
Lorem ipsum content
unused IDs
unused CSS classes
old image filenames
```

Do not delete files based only on their filenames.

Confirm that they are unused and unrelated before removing them.

Create a brief deletion report showing:

* File removed
* Why it appeared unrelated
* Whether it had any code references
* Why deletion was considered safe

## 8. Reorganise Images and Videos

Move all active image and video assets into a consistent directory structure under:

```text
assets/img/
```

Recommended structure:

```text
assets/
  img/
    hero/
    logo/
    sections/
    services/
    projects/
    testimonials/
    icons/
    backgrounds/
    gallery/
    video/
```

Examples:

```text
assets/img/hero/home-hero.webp
assets/img/logo/site-logo.svg
assets/img/sections/about-team.webp
assets/img/services/service-consulting.webp
assets/img/testimonials/client-name.webp
assets/img/video/company-introduction.mp4
```

The exact folders should reflect the actual content of the site.

Do not create empty folders that have no purpose.

Use clear, descriptive filenames.

Prefer:

```text
home-hero.webp
service-web-design.webp
client-jane-smith.webp
company-logo-dark.svg
```

Avoid:

```text
IMG_2938.jpg
final-final2.png
image1.jpg
new-banner-copy.jpg
```

## 9. Update All Asset References

After moving assets, update every reference in:

* HTML
* Markdown
* Liquid templates
* Jekyll includes
* Layouts
* Front matter
* Sass
* CSS
* JavaScript
* JSON
* YAML data files
* `_config.yml`

Use Jekyll-compatible paths where appropriate.

Example:

```liquid
{{ "/assets/img/hero/home-hero.webp" | relative_url }}
```

For front matter or data files:

```yaml
image: /assets/img/hero/home-hero.webp
```

Ensure moved assets do not create broken paths.

## 10. Image Optimisation

During the asset audit:

* Identify duplicate images
* Identify oversized source images
* Identify unused image variants
* Identify unnecessary PNG files
* Prefer SVG for simple logos and icons
* Prefer WebP or AVIF for photographic images where supported
* Preserve original assets where conversion could reduce quality or compatibility
* Add meaningful alt text in markup or data
* Add width and height attributes where possible
* Use lazy loading for below-the-fold images

Example:

```html
<img
  src="{{ '/assets/img/sections/about-team.webp' | relative_url }}"
  alt="The company team working together"
  width="1200"
  height="800"
  loading="lazy"
>
```

Do not lazy-load the primary above-the-fold hero image if that would harm Largest Contentful Paint.

## 11. Video Organisation

Move active videos into:

```text
assets/img/video/
```

Although videos are not images, use this location because the requested project convention places all visual media under `assets/img`.

Organise and rename videos clearly.

Check:

* File size
* Format
* Autoplay behaviour
* Muted state
* Loop behaviour
* Mobile fallback
* Poster image
* Accessibility
* Reduced-motion handling

Provide a poster image where appropriate.

Example:

```html
<video
  class="hero__video"
  autoplay
  muted
  loop
  playsinline
  poster="{{ '/assets/img/hero/home-video-poster.webp' | relative_url }}"
>
  <source
    src="{{ '/assets/img/video/home-hero.mp4' | relative_url }}"
    type="video/mp4"
  >
</video>
```

## 12. Centralise Shared Component Styling

Each reusable component should have one clear SCSS source of truth.

For example:

```text
_sass/components/_button.scss
_sass/components/_card.scss
_sass/components/_hero.scss
```

Do not define separate button styles in multiple page files.

Do not duplicate card styling across unrelated sections.

Use modifiers for controlled variations:

```scss
.button {}
.button--primary {}
.button--secondary {}
.button--small {}
.button--full-width {}
```

```scss
.card {}
.card--featured {}
.card--horizontal {}
.card--dark {}
```

Use page-specific files only for layout or behaviour that is genuinely unique to that page.

## 13. Avoid One Giant Stylesheet

Centralise design decisions, but do not place the entire website’s CSS in one oversized file.

The central variables or token file should contain shared values.

Component styles should remain in focused partials.

The main Sass entry file should only load the architecture.

Example:

```scss
@use "abstracts";
@use "base";
@use "layout";
@use "components";
@use "pages";
@use "utilities";
```

The goal is one source of truth, not one unmanageable file.

## 14. Dead Code Audit

Identify and safely remove:

* Unused SCSS partials
* Unused CSS classes
* Unused mixins
* Unused variables
* Unused layouts
* Unused includes
* Unused data files
* Unused JavaScript
* Unused images
* Unused videos
* Unused fonts
* Empty directories
* Backup files
* Temporary files

Search the entire project before deletion.

Be careful with classes created dynamically by Liquid or JavaScript.

Do not remove code merely because a basic text search returns no result.

## 15. Preserve Git History Where Practical

When reorganising files:

* Prefer moving files rather than deleting and recreating them
* Use file moves that Git can recognise as renames
* Avoid combining unrelated refactors into one change
* Keep asset moves separate from styling changes where practical
* Make the changes easy to review

## 16. Validation

After each major refactor:

1. Run the Jekyll build.
2. Check for Sass compilation errors.
3. Check for Liquid errors.
4. Check for broken internal links.
5. Check for missing images and videos.
6. Check for missing fonts.
7. Check for JavaScript selector breakage.
8. Review key pages on desktop and mobile.
9. Compare the refactored site against the original.
10. Confirm that the visual appearance and functionality remain intact.

Use a broken-link or asset-reference check where available.

## Required Working Method

Work in phases.

### Phase 1: Audit

Produce a short report showing:

* Current Sass structure
* Current class naming issues
* Duplicate styles
* Active fonts
* Suspected orphaned fonts
* Current asset locations
* Suspected files from other websites
* Unused or duplicate images
* High-risk areas

Do not delete anything during the initial audit.

### Phase 2: Proposed Structure

Present:

* Proposed Sass folders
* Proposed BEM names
* Proposed design-token file
* Proposed asset structure
* Proposed file moves
* Proposed deletion candidates

### Phase 3: Refactor

Apply the changes incrementally:

1. Create or clean the design tokens.
2. Restructure the Sass architecture.
3. Convert components to BEM.
4. Update markup class names.
5. Reorganise images and video.
6. Update all asset references.
7. Consolidate fonts.
8. Remove confirmed orphaned files.
9. Remove confirmed remnants from other sites.
10. Run final validation.

### Phase 4: Final Report

Provide:

* Files added
* Files moved
* Files renamed
* Files removed
* BEM components introduced
* Old classes replaced
* Design tokens centralised
* Duplicate styles removed
* Fonts retained
* Fonts removed
* Assets reorganised
* Remnants from other sites removed
* Build result
* Remaining risks or manual checks

## Important Safety Rules

* Do not redesign the site.
* Do not remove files without checking references.
* Do not remove fonts solely because they appear old.
* Do not break Liquid-generated class names.
* Do not change public URLs unless required.
* Do not change content unnecessarily.
* Do not introduce a CSS framework.
* Do not add Material UI as a dependency.
* Do not replace working components simply to make the code look different.
* Preserve accessibility and improve it where possible.
* Preserve all responsive behaviour.
* Keep a clear record of deletions and file moves.

The final result should be a visually unchanged but significantly cleaner Jekyll website with consistent BEM naming, centralised design tokens, organised assets, fewer orphaned files and a maintainable Sass architecture.
