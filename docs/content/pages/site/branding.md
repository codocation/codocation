---
title: Titles, URLs, and Branding
---

The reader-facing identity of a site lives in its entry in `codocation.yml`.

## Title and description

```yaml
sites:
  cdc:
    title: Codocation
    description: Author documentation in your IDE and ship it as a static site or PDF.
```

The title names the site in the header, the browser tab, and the PDF cover. The description
is a one-line summary shown in the site footer and used as the site's summary in `llms.txt`.
It does not feed a page's meta description; a page without its own `seo-description` emits
no `<meta name="description">` at all. Both fields are also editable from "Edit
Documentation..." in the "Codocation" tool window's gear menu; see
[Navigation](../writing/navigation.md).

## URLs

- **Public URL** (`publicUrl`) - the public origin of the site (for example
  `https://docs.example.com`). It feeds absolute URLs in `sitemap.xml` (the sitemap protocol
  requires them), the `Sitemap:` line of `robots.txt`, and `llms.txt`/`llms-full.txt` (a
  language model reads those files detached from the site, so relative paths resolve to
  nothing). The rendered pages themselves work without it - internal navigation is relative.
  Set it in the "Web" tab of the "Site & Export" tool window, in the "Branding" section next
  to Home URL.
- `basePath` - the mount path when the site is not served at the domain root (for example
  `blog` for `https://example.com/blog/`). Two sites in one project must have different
  base paths.
- `homeUrl` - an explicit target for the header "home" link, when clicking the site name
  should lead somewhere other than the site's own root (for example from docs back to the
  product page).

## Logo

```yaml
sites:
  cdc:
    web:
      branding:
        logo: images/logo.svg
        logoAlt: Codocation
```

The logo renders in the site header next to the title; `logoAlt` is its accessible alt
text. The file is copied into the site assets automatically.

## Font

```yaml
sites:
  cdc:
    web:
      branding:
        font: assets/fonts/Inter-Regular.woff2
```

The site font is set from a font file (`woff2`, `woff`, `ttf`, or `otf`) under
`content/assets/fonts/`, picked in the "Branding" section of the "Web" tab. The build copies
the file into the site and emits a matching `@font-face` rule, then applies it as the body and
heading font - the font renders for every reader, not only those who already have it
installed. `woff2` gives the smallest download. A file picked from outside
`content/assets/fonts/` is copied in; if a file with the same name is already there, the
picker asks whether to keep both, overwrite, or cancel.

## Custom CSS and JavaScript

```yaml
web:
  customization:
    css: assets/site.css
    js: assets/site.js
```

`customization.css` and `customization.js` point to files under `content/assets/` that are
linked into every page of the site.

## Themes

Every site ships with a light and a dark theme and a header toggle; the reader's choice is
remembered. The docs and blog site types each have their own visual theme, applied
automatically from the site's `type`.

## External links

By default, external (`http`/`https`) links open in a new tab. Set
`externalLinksNewTab: false` on the site to keep readers in the same tab instead.
