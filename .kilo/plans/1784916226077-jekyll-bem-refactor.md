# evo Bond Legacy Jekyll Refactor Plan

## 1. What I read and confirmed

- `docs/seo-guru.md` footer spec requires a two-tier YAML-driven footer from `sitetext.yml`.
- Current `_data/sitetext.yml` already has most section copy under `header`, `services`, `calculators`, `more-calculators`, `textblock`, `about`, `clients`, `team`, `testimonials`, `contact`, `portfolio`, `footer`.
- Hardcoded text still exists in: `articles.html`, `textblock.html`, `team.html`, `about.html`, `footer.html`, `navheader.html`, `contact.md`, `404.html`.
- `_assets/site.scss` compiles via webpack to `assets/bundle.css` and `assets/bundle.js` — keep this pipeline intact.
- No `@import` -> `@use` migration (Bootstrap 4 + current SCSS loader still use `@import`).

## 2. Decisions to resolve before executing

**Q:** For "all text in sitetext.yml" — should every static text string move into YAML, including footer sub-labels like "Privacy Statement", "Data Sharing", WhatsApp "Chat", copyright template, and legal link text?

**My recommendation:** Yes. Dynamic values (copyright year `{{ 'now' | date: "%Y" }}`, `site.company`, `site.contact`) stay in Liquid. Every literal string moves to `_data/sitetext.yml`. This keeps the site fully editable via data without touching includes.

## 3. Step-by-step execution plan

A. **Design tokens cleanup** (`_assets/base/_variables.scss`)
   - Standardize naming to `$color-*`, `$font-*`, `$space-*`, `$shadow-*` where missing.
   - Fix `$pace-lg` typo in `_services.scss:30` → `$space-lg`.
   - Remove `$button-radius: 30px` conflict with `$btn-radius: $radius-md` (keep one, use `$radius-pill` if pill buttons needed).

B. **Sass restructuring** (rename/move files, update `site.scss` imports)
   - `_assets/components/_navbar.scss` → `_assets/components/_navigation.scss`
   - `_assets/layout/_masthead.scss` → `_assets/layout/_header.scss`
   - Combine section-heading rules from `_page.scss` → new `_assets/layout/_section.scss`
   - Extract `.card` rules from `_services.scss` → `_assets/components/_card.scss` with BEM
   - Extract `.team-member` from `_team.scss` → keep as `.team-card` with BEM
   - Extract calculator iframe styles from `_page.scss` → `_assets/layout/_calculators.scss`
   - Empty `_assets/layout/dist/` directory → delete
   - Fix `_components/client-scroll.scss` invalid selector

C. **BEM class conversions** (Sass + markup together)
   - `.masthead` → `.hero`
   - `.intro-text` → `.hero__content`
   - `.intro-lead-in` → `.hero__lead`
   - `.intro-heading` → `.hero__title`
   - `.page-section` → `.section`
   - `.section-heading` → `.section__heading`
   - `.section-subheading` → `.section__subheading`
   - `.textblock-subheading` → `.section__subheading` (same, unify)
   - `.service-heading` → `.service-card__title`
   - `.card` (services/calculators) → `.service-card`
   - `.card-body` → `.service-card__body`
   - `.card-footer` → `.service-card__action`
   - `.team-member` → `.team-card`
   - `.card-title` → `.service-card__title`
   - `.textblock-heading` → `.textblock__title`
   - `.textblock-image` → `.textblock__icon`
   - `.textblock-title` → `.textblock__item`
   - `.portfolio-item` → `.portfolio-card`
   - `.portfolio-link` → `.portfolio-card__link`
   - `.portfolio-hover` → `.portfolio-card__overlay`
   - `.portfolio-hover-content` → `.portfolio-card__overlay-content`
   - `.portfolio-caption` → `.portfolio-card__caption`
   - `.modal` → `.modal` (keep, but BEM inside: `.modal__dialog`, etc.)
   - `.close-modal` → `.modal__close`
   - Keep `#mainNav` ID for JS/scrollspy; update class to `.nav__list`, `.nav__item`, `.nav__link`
   - `.whatsapp` → `.whatsapp` (BEM block, no elements needed)
   - `.social-buttons` → `.social-links`
   - `.highlight` → `.highlight` (keep, or `.text-highlight`)

D. **Data migration** (move hardcoded text → `_data/sitetext.yml`)
   - `articles.html` hardcoded "Articles" → `sitetext.articles.*`
   - `textblock.html` has defaults already in sitetext; ensure nothing hardcoded remains
   - `team.html` heading default already handled; move any remaining text
   - `about.html` "About Us" default → `sitetext.about.title` (already present)
   - `footer.html` → expand `sitetext.footer` with description, copyright template, privacy/data link text, legal text, social links, sections (navigation columns for seo-guru.md two-tier layout)
   - `navheader.html` → uses `sitetext.header.*` already, no change needed
   - `contact.md`, `404.html` → can reference sitetext for headings if not already

E. **Footer rebuild** (adopt seo-guru.md two-tier pattern)
   - Expand `_data/sitetext.yml` → `footer:` with:
     - `description`
     - `copyright_text`
     - `legal_text`
     - `privacy_link_text`
     - `data_sharing_link_text`
     - `sections:` (array of nav-column objects: title + links)
     - `social:` (already exists)
   - Rebuild `_includes/footer.html` to render two tiers:
     1. Main area: brand/description + section nav columns + social
     2. Bottom strip: copyright + legal links
   - Move WhatsApp from after-footer position to inside footer main area (or keep as floating, document decision)
   - Remove inline fixed-footer script (use CSS `position: sticky` or remove entirely; if layout fix needed, move to `site.js`)

F. **Asset reorganization**
   - Move `assets/evo Bond Legacy Logo.png` → `assets/img/logo/evo-bond-legacy-logo.png` (rename, remove spaces)
   - Move `_assets/img/contact.png` → `assets/img/contact/contact-bg.png`
   - Move `_assets/img/evo-bond-legacy-coming-soon.jpg` → `assets/img/hero/hero-bg.jpg`
   - Move `assets/img/team/Thea Sauer evo Bond Legacy.jpg` → `assets/img/team/thea-sauer.jpg` (rename, remove spaces)
   - Move `assets/img/portfolio/franchises.jpg`, `insurance.jpg` → keep in `assets/img/portfolio/`
   - Move `assets/img/clients/*` → keep in `assets/img/clients/`
   - Update all references: `_config.yml`, `_assets/*.scss`, `sitetext.yml`, any includes
   - Remove `evo_up_homeloan_experts.gif`, `favicon(old).png`, `social_916x509.jpg` reference → move/convert to `assets/img/social/social-cover.webp` or keep as JPEG
   - Remove all hash-named font/image artifacts in `assets/` root (unreferenced webpack output)
   - Remove `.lnk` files
   - Remove `_assets/bundle.js` (it's webpack entry, source stays; but it's a build artifact — check if needed in repo)

G. **Dead code removal**
   - `docs/` directory — remove 5 prompt/spec files that are not part of the site
   - `_assets/layout/dist/` — empty, remove
   - `_drafts/` — 2 draft posts not in collection; review, recommend removal
   - `_portfolio/` — 2 example files used by portfolio collection; keep (referenced by modals)
   - `package.json`, `package-lock.json`, `webpack.config.js` — keep (they describe the existing build pipeline)
   - Root-level gifs/pngs that are unreferenced after move — remove

H. **Validation**
   - Run `jekyll build` and confirm no errors
   - Check Sass compilation via `webpack` or equivalent
   - Audit all internal links
   - Verify all images render (compare asset paths in includes vs filesystem)
   - Review footer renders correctly with new YAML data
   - Review key pages: home, contact, 404, blog post

## 4. Risks / caveats
- Footer restructure changes HTML structure — must verify responsive behaviour.
- Asset path moves require updating `_config.yml` logo path and all SCSS `url()` references.
- BEM rename of `.section-heading` to `.section__heading` touches every include + CSS; one miss breaks styling.
- Removing hash-named font files requires confidence they are truly unreferenced (confirmed: no `@font-face`, no SCSS/CSS/JS references found).

## 5. Open question

**Q:** Should every literal string move into `_data/sitetext.yml`, including tiny footer sub-labels (Privacy Statement, Data Sharing, "Chat", copyright format), or only the section-level copy?

**My recommendation:** Move all static text into `sitetext.yml`. Only keep dynamic Liquid (`{{ 'now' | date: "%Y" }}`, `{{ site.company }}`) in markup.
