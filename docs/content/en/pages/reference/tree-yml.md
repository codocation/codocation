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
    icon: github               # draw the entry as a brand mark
    label: GitHub              # its tooltip and accessible name
  - href: https://acme.com/signup
    label: Get started
    button: primary            # draw it as a CTA button: primary is filled, secondary outlined
    color: "#6c5ce7"           # optional: the fill of a primary, the border of a secondary
toc:                           # the sidebar / table of contents
  - page: pages/index.md
    home: true                 # served at the site root; exactly one page may have it
  - page: pages/getting-started.md
    title: Start Here          # optional label override; default = the page's own title
  - section:                   # a named group, not a page; the key itself takes no value
    id: guides                 # its stable identity, which locale trees match on
    label: Guides              # what the sidebar shows
    collapsed: true            # start collapsed in the sidebar
    children:
      - page: pages/guides/install.md
      - page: pages/guides/deploy.md
  - page: pages/reference.md
    children:                  # child refs resolve relative to the parent page's directory
      - page: reference/api.md
footer:                        # the four parts the theme paints, each in its own slot
  nav:                         # plain link rows; same item forms as header
    - href: https://acme.com/support
      label: Support
  social:                      # brand marks, each naming its brand as its own key
    - github: https://github.com/acme/product
  legal:                       # the centered row above the copyright line
    - label: Privacy
      href: https://acme.com/privacy
  copyright: © 2026 Acme Inc.  # omit for the built-in line; "" prints nothing at all
```

## Site metadata

`title` is required and is the only source of the locale's site name shown in the header, footer,
browser metadata, and generated artifacts. `description` is optional. Both fields are
localizable and therefore belong to the tree rather than `codocation.yml`.

## TOC titles

The optional page-item `title` is a TOC-only label. Editing it changes the sidebar and navigation
text for that item and never changes the Markdown page title, frontmatter bundle, or rendered H1. A
valid `title` never produces an invalid-field diagnostic.

## How a locale tree inherits

A locale's tree is a sparse overlay on the site's effective default language. Every key it leaves
out is inherited, and only the keys it writes are its own, so a German tree that translates two
section labels is two sections long and takes everything else from English.

A list the locale does not write is inherited whole. A list it does write replaces membership
outright, `[]` included, while the nodes inside that list still resolve their own remaining keys
against their counterparts. Leaving a page out of a locale tree therefore does not exclude it; a
locale excludes a page by writing the list without it.

Nodes are matched across languages by identity alone, never by position: `page:` for a page, `href:`
for a link, `id:` for a section. Matching never crosses sections, and a node whose parent found no
counterpart has none either. A duplicated identity on either side makes both ambiguous, and they
inherit nothing. One consequence is worth stating outright: a locale cannot point a link at a
localized URL, because changing `href` changes which node it is.

`footer:` inherits one slot at a time, so a locale may translate its `legal` links while `nav`,
`social` and `copyright` stay the default language's. A bare `footer:` writes the map without
writing any slot key, so every slot still inherits.

An entry that is this language's own rather than a translation of anything carries
`inherited: false`. Every item kind accepts it, it defaults to true, and it is an error in the
default-language tree, where every item is already that language's own. An entry that claims a
translation and finds no counterpart is a warning, and that marker is how you answer it.

## Where a page's text comes from

Not from the tree. A page resolves from the requested locale's own file under `pages/` when that
file exists, and from the site's effective default language when it does not. There is no marker for
it, because "does this entry translate one in the default language" and "where does this page's
markdown come from" are two different questions, and the tree answers only the first. A page with no
file in the requested locale is not a diagnostic: that is the ordinary state of an untranslated page
and of every single-locale project.

The older `translation` key, its `excluded` state and the `reason` field beside it are all rejected
outright.

## Item forms

- **Page**: `page:` with a canonical logical path under `pages/` when it has no ancestor page,
  whether it is at the root or inside sections. In that case, exactly one bare Markdown filename
  such as `index.md` is also accepted as a fallback for `pages/index.md`. Any descendant of a page
  resolves relative to the nearest ancestor page's directory, even through intervening sections,
  so the `reference/api.md` child above resolves to `pages/reference/api.md` when the parent is
  `pages/reference.md`. A child `api.md` under a parent such as `pages/guides/reference.md` would
  resolve to `pages/guides/api.md`. Optional keys: `title` (label override), `home` (site root marker),
  `hidden` (keep the entry out of the published navigation), `children` (nested items), and
  `inherited: false` (this entry is this language's own, not a translation).
- **External link**: `href:` with `label:`. A header link may add `icon:` to draw it as a brand mark,
  or `button:` to draw it as a CTA: `primary` for the filled button, `secondary` for the outlined one,
  the same two names an inline `[Text](url){button="primary"}` uses on a page. `color:` goes with
  `button:` and states your own shade where the theme's accent would stand, so it is the fill of a
  primary and the border of a secondary.
- **Section**: `section:` with no value of its own, plus the siblings `id:`, `label:` and
  `children:`; sections group pages in the `toc` and may nest. Optional `collapsed: true`. The `id`
  is the section's stable identity and must be unique in the file; the `label` is what readers see,
  so renaming a section leaves every other file pointing at it alone.

## Rules validation enforces

- The default-language tree must declare `title`, `header`, `toc` and `footer`. It inherits from
  nothing, so it has to be complete; a locale tree may omit all four, which simply means the whole
  tree is inherited.
- `inherited` is an ERROR in the default-language tree. In a locale tree, an entry with no
  counterpart in the default language is a WARNING until `inherited: false` says it is this
  language's own.
- Two sections in one file may not share an `id`.
- Pair trees are required only for declared site memberships. A tree or sidecar for an undeclared
  pair is an ERROR and ignored; a locale directory absent from the root catalog is an orphan-locale
  ERROR and ignored.
- The root `title` must be non-empty.
- Only one page carries `home: true`.
- `header` is a flat list and `footer.nav` a flat list: no nested sections in either.
- `footer` is a map of `nav`, `social`, `legal` and `copyright`, not a list. The order between those
  four belongs to the theme, so no authored order can put a brand mark above a nav row; the order
  inside a slot is yours.
- An item is one thing: `page`, `section`, or `href`, never a combination.
