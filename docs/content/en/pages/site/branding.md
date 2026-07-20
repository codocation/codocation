---
title: Titles, URLs, and Branding
---

The reader-facing title and description live in `content/<locale>/<siteId>.tree.yml`.
Technical URL and global web settings live in `codocation.yml`; locale-aware branding images
are resolved from the requested locale and its permitted fallback.

## Title and description

```yaml
# content/en/cdc.tree.yml
title: Codocation
description: Author documentation in your IDE and ship it as a static site or PDF.
header: []
toc: []
footer: []
```

The required title names the site in the header, browser tab, and PDF cover. The description
is a one-line summary shown in the site footer and used as the site's summary in `llms.txt`.
It does not become a page's meta description. Both fields are editable from "Edit
Documentation..." in the "Codocation" tool window's gear menu; see
[Navigation](../writing/navigation.md).

## URLs

- **Public URL** (`publicUrl`) is the public origin of the site. It feeds absolute URLs in
  `sitemap.xml` (the sitemap protocol requires them), the `Sitemap:` line of `robots.txt`, and
  `llms.txt`/`llms-full.txt` (a language model reads those files detached from the site, so
  relative paths resolve to nothing). Rendered pages themselves work without it because
  internal navigation is relative. Set it in the "Web" tab of the "Site & Export" tool
  window, in the "Branding" section next to Home URL.
- **`basePath`** is the mount path when the site is not served at the domain root.
- **`homeUrl`** is an explicit target for the header home link when it should leave the site.

## Logo and favicon

```yaml
sites:
  cdc:
    web:
      branding:
        logo: images/branding/logo.svg
        favicon: assets/media/favicon.ico
```

The logo is a locale-owned image. Its accessible alt text comes from the required tree title.
An English fallback page displayed in another locale keeps the page's source provenance when
resolving its images and attachments, so it cannot accidentally use a translated asset from
the requested locale. The favicon is explicitly non-localized and must remain under the
global `assets/media/` namespace.

## Fonts, CSS, and JavaScript

```yaml
web:
  branding:
    font: assets/fonts/Inter-Regular.woff2
  customization:
    css: assets/css/site.css
    js: assets/js/site.js
```

Font binaries, CSS, JavaScript, and analytics scripts are project-global and must live under
their matching enforced `assets/` subnamespace. This layout has no locale-specific font,
favicon, CSS, JavaScript, or analytics resource. The site font is selected from a font file
(`woff2`, `woff`, `ttf`, or `otf`) under `assets/fonts/`, copied into the site, and emitted in
a matching `@font-face` rule before it is applied to body and heading text. `woff2` gives the
smallest download. When the picker starts with a file outside `assets/fonts/`, it copies the
file into that global namespace; if the same filename is already there, it asks whether to
keep both, overwrite, or cancel. CSS and JavaScript are linked into every page when configured.

## Themes and external links

Every site ships with a light and a dark theme and a header toggle; the reader's choice is
remembered. The docs and blog site types each have their own visual theme, applied
automatically from the site's `type`. By default, external (`http`/`https`) links open in a
new tab; set `externalLinksNewTab: false` on the site to keep readers in the same tab.
