---
title: <site id>.tree.yml
---

The navigation file of a site and locale, at `content/<locale>/<site id>.tree.yml`. The
"Codocation" tool window edits it visually; the format below is what lands in the file.
The physical file `content/en/cdc.tree.yml` contains the English tree, while its page entries
use logical paths such as `pages/guide.md`, without the physical `content/en/` prefix. The
configured language is registered under the `locales:` key in `codocation.yml`.

```yaml
title: Product Documentation  # required, reader-facing site name
description: Guides for using the product. # optional site summary
header:                        # links in the site header
  - page: pages/pricing.md
  - href: https://github.com/acme/product
    label: GitHub
    button: true               # render as a button instead of a plain link
    icon: github               # optional icon
toc:                           # the sidebar / table of contents
  - page: pages/index.md
    home: true                 # served at the site root; exactly one page may have it
  - page: pages/getting-started.md
    title: Start Here          # optional label override; default = the page's own title
  - section: Guides            # a named group, not a page
    collapsed: true            # start collapsed in the sidebar
    children:
      - page: pages/guides/install.md
      - page: pages/guides/deploy.md
  - page: pages/reference.md
    children:                  # pages can nest children directly, without a section
      - page: pages/reference/api.md
footer: []                     # links in the site footer; same item forms as header
```

## Site metadata

`title` is required and is the only source of the locale's site name shown in the header, footer,
browser metadata, and generated artifacts. `description` is optional. Both fields are
localizable and therefore belong to the tree rather than `codocation.yml`.

## Item forms

- **Page**: `page:` with a canonical logical path under `pages/`. Optional keys: `title` (label
  override), `home` (site root marker), `hidden` (keep the entry out of the published
  navigation), `children` (nested items), and `translation` (`translated` or `fallback`).
- **External link**: `href:` with `label:`; header/footer links may add `button: true`,
  `color:`, and `icon:`.
- **Section**: `section:` with a label and `children:`; sections group pages in the `toc`
  and may nest. Optional `collapsed: true`.

## Rules validation enforces

- A `translated` page must exist physically in the requested locale; a `fallback` page must
  resolve to an existing default-locale source.
- A page without `translation` is `translated`. `translation: translated` requires the physical
  page in the requested locale; it never silently falls back. `translation: fallback` is valid
  only in a non-default locale and resolves that logical page from the default locale. A
  fallback marker in the default tree, or a fallback marker next to a requested-locale file,
  is an ERROR diagnostic. A fallback marker whose default source is absent is a reader failure.
- Omitting a page from a locale tree excludes it from that locale; there is no exclusion
  marker and no tree fallback.
- The root `title` must be present and non-empty.
- Only one page carries `home: true`.
- `header` and `footer` are flat: no nested sections there.
- An item is one thing: `page`, `section`, or `href`, never a combination.
