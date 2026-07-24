---
title: <site id>.tree.yml
---

The navigation file of a declared site and locale pair, at `content/<locale>/<site id>.tree.yml`. The
"Codocation" tool window edits it visually; the format below is what lands in the file.
The physical file `content/en/cdc.tree.yml` contains the English tree, while its page entries
use logical paths such as `pages/guide.md`, without the physical `content/en/` prefix. The
locale code is registered in the project-wide `locales:` catalog, and the pair is published only
when that code appears in `sites.<siteId>.locales`.

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
    children:                  # child refs resolve relative to the parent page's directory
      - page: reference/api.md
footer: []                     # links in the site footer; same item forms as header
```

## Site metadata

`title` is required and is the only source of the locale's site name shown in the header, footer,
browser metadata, and generated artifacts. `description` is optional. Both fields are
localizable and therefore belong to the tree rather than `codocation.yml`.

## TOC titles and fallback pages

The optional page-item `title` is a TOC-only label. Editing it changes the sidebar/navigation text
for that item and never changes the Markdown page title, frontmatter bundle, or rendered H1. A
non-default locale may combine this localized TOC title with `translation: fallback`: navigation is
localized while page content resolves from the site's `defaultLocale`. The marker enables fallback
but does not select the source. The fallback entry must have an existing source and is forbidden in
the effective default locale. A missing source is an ERROR with a quick fix; a valid `title` does
not produce an invalid-field diagnostic.

## Item forms

- **Page**: `page:` with a canonical logical path under `pages/` when it has no ancestor page,
  whether it is at the root or inside sections. In that case, exactly one bare Markdown filename
  such as `index.md` is also accepted as a fallback for `pages/index.md`. Any descendant of a page
  resolves relative to the nearest ancestor page's directory, even through intervening sections,
  so the `reference/api.md` child above resolves to `pages/reference/api.md` when the parent is
  `pages/reference.md`. A child `api.md` under a parent such as `pages/guides/reference.md` would
  resolve to `pages/guides/api.md`. Optional keys: `title` (label override), `home` (site root marker),
  `hidden` (keep the entry out of the published navigation), `children` (nested items), and
  `translation` (`translated` or `fallback`).
- **External link**: `href:` with `label:`; header/footer links may add `button: true`,
  `color:`, and `icon:`.
- **Section**: `section:` with a label and `children:`; sections group pages in the `toc`
  and may nest. Optional `collapsed: true`.

## Rules validation enforces

- A `translated` page must exist physically in the requested locale; a `fallback` page must
  resolve to an existing source in the site's effective `defaultLocale`.
- A page without `translation` is `translated`. `translation: translated` requires the physical
  page in the requested locale; it never silently falls back. `translation: fallback` is valid
  only in a non-default site locale and resolves that logical page from the site's
  `defaultLocale`. A fallback marker in the effective default tree, or a fallback marker next to a requested-locale file,
  is an ERROR diagnostic. A fallback marker whose source is absent is a reader failure.
- Omitting a page from a locale tree excludes it from that locale; there is no exclusion
  marker and no tree fallback.
- Pair trees are required only for declared site memberships. A tree or sidecar for an undeclared
  pair is an ERROR and ignored; a locale directory absent from the root catalog is an orphan-locale
  ERROR and ignored.
- The root `title` must be present and non-empty.
- Only one page carries `home: true`.
- `header` and `footer` are flat: no nested sections there.
- An item is one thing: `page`, `section`, or `href`, never a combination.
