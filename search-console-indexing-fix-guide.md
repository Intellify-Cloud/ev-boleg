# Search Console Indexing Fix Guide

Use this checklist when Google Search Console reports pages as:

- `Alternate page with proper canonical tag`
- `Page with redirect`
- `Crawled - currently not indexed`
- `URL is not on Google`

## 1. Confirm The Preferred URL

Pick one official version of the site URL and use it everywhere.

Preferred examples:

```text
https://example.com/
https://example.com/contact/
```

Avoid mixing these unless redirects and canonicals are deliberate:

```text
http://example.com/
http://www.example.com/
https://www.example.com/
https://example.com/page.html
https://example.com/page
https://example.com/page/
```

For most sites, the cleanest setup is:

```text
https://example.com/page/
```

or:

```text
https://example.com/page
```

Choose one style and be consistent.

## 2. Check The Live Canonical Tag

Open the page in your browser.

Example:

```text
https://example.com/privacy-statement
```

Then open page source:

```text
Ctrl + U
```

Search:

```text
canonical
```

The canonical should point to the final preferred version of that same page.

Good:

```html
<link rel="canonical" href="https://example.com/privacy-statement">
```

Bad:

```html
<link rel="canonical" href="https://www.example.com/privacy-statement">
<link rel="canonical" href="https://example.com/privacy-statement.html">
<link rel="canonical" href="https://old-domain.com/privacy-statement">
```

If the canonical points to another domain, to `www` when the site uses non-`www`, or to `.html` when links use clean URLs, fix the site template/config.

## 3. Check Redirects

Open each alternate URL in the browser and see where it lands.

Test:

```text
http://www.example.com/
https://www.example.com/
http://example.com/
https://example.com/page.html
```

They should redirect to the preferred version.

Example:

```text
http://www.example.com/
```

should become:

```text
https://example.com/
```

If old or alternate URLs redirect correctly, Search Console may list them under `Page with redirect`. That is usually fine. Redirected URLs are not supposed to be indexed.

## 4. Fix Jekyll Canonicals

In Jekyll, check `_config.yml`.

Good:

```yml
url: https://example.com
baseurl: ""
```

Bad if the real site is not on that domain/path:

```yml
url: https://old-domain.com/some-folder/
```

Then check the head include, often:

```text
_includes/head.html
```

Good canonical logic:

```liquid
{% assign canonical_url = page.url | replace:'index.html','' | absolute_url %}
<link rel="canonical" href="{{ canonical_url }}">
```

If the site uses clean URLs but Jekyll creates `.html` URLs, normalize the canonical:

```liquid
{% assign canonical_path = page.url | replace:'index.html','' | replace:'.html','' %}
{% assign canonical_url = canonical_path | absolute_url %}
<link rel="canonical" href="{{ canonical_url }}">
```

Also align social tags:

```liquid
<meta property="og:url" content="{{ canonical_url }}">
<meta property="twitter:url" content="{{ canonical_url }}">
<meta property="og:image" content="{{ '/assets/img/social/social-cover.webp' | absolute_url }}">
<meta property="twitter:image" content="{{ '/assets/img/social/social-cover.webp' | absolute_url }}">
```

## 5. Fix Clean URL Mismatches

If internal links use:

```text
/privacy-statement
```

but generated pages/canonicals use:

```text
/privacy-statement.html
```

choose one style.

For clean Jekyll URLs, add explicit permalinks to pages:

```md
---
layout: page
title: Privacy Policy
permalink: /privacy-statement/
---
```

Then update internal links to match:

```text
/privacy-statement/
```

## 6. Check Search Console After Deploy

Do this only after the fix is deployed.

In Search Console:

1. Open **URL Inspection**.
2. Test the preferred URL.
3. Click **Test Live URL**.
4. Confirm it says the page can be indexed.
5. Click **Request Indexing**.
6. Go back to the issue report.
7. Click **Validate Fix**.

If Search Console still shows old canonical data, check the crawl date. If the last crawl was before the deployment, Google is showing stale information.

## 7. How To Interpret Common Reasons

### Alternate page with proper canonical tag

Google found a duplicate URL and chose another canonical.

This is okay if the affected URL is an alternate, such as:

```text
https://www.example.com/page
https://example.com/page.html
```

This is bad if the preferred page itself is excluded because its canonical points somewhere else.

Fix:

- Make canonical self-referential on the preferred page.
- Redirect alternate domains and URL formats to the preferred URL.
- Update internal links to preferred URLs.

### Page with redirect

Google found a URL that redirects.

Usually fine:

```text
http://www.example.com/
```

redirects to:

```text
https://example.com/
```

Fix only if the redirect points to the wrong page or if the preferred URL is redirecting unexpectedly.

### Crawled - currently not indexed

Google crawled the page but has not indexed it.

Common causes:

- Thin content
- Duplicate content
- Mostly iframe content
- Low internal linking
- Recently discovered page
- Weak page title/meta content

Fix:

- Add unique, useful text content.
- Make sure the page has one clear heading.
- Add internal links to the page.
- Ensure canonical points to itself.
- Request indexing after deploy.

## 8. Quick Per-Page Checklist

For each affected URL:

```text
[ ] Does it load?
[ ] Does it redirect? If yes, is the destination correct?
[ ] Does the final URL use HTTPS?
[ ] Does the final URL use the preferred www/non-www version?
[ ] Does the canonical point to the final preferred URL?
[ ] Do internal links point to the preferred URL?
[ ] Is the page in the sitemap?
[ ] Did Search Console last crawl it after the fix was deployed?
[ ] Did you run Test Live URL?
[ ] Did you request indexing?
```

## 9. What Not To Worry About

These are usually not problems:

- `http` URLs excluded because they redirect to `https`
- `www` URLs excluded because they redirect to non-`www`
- `.html` URLs excluded because clean URLs are canonical
- Old URLs appearing in reports for a while after a fix

The important thing is that the final preferred URL is indexable and has a self-referential canonical.
