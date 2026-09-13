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
    type: github                # draw the entry as a brand mark
    title: GitHub                # its tooltip and accessible name
  - href: https://acme.com/signup
    title: Get started
    type: primary-button        # draw it as a CTA button: primary is filled, secondary outlined
    color: "#6c5ce7"             # optional: the fill of a primary, the border of a secondary
toc:                           # the sidebar / table of contents
  - page: pages/index.md
    home: true                 # served at the site root; exactly one page may have it
  - page: pages/getting-started.md
    title: Start Here          # optional label override; default = the page's own title
  - section:                   # a named group, not a page; the key itself takes no value
    id: guides                 # its stable identity, which locale trees match on
    title: Guides               # what the sidebar shows
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
      title: Support
  social:                      # brand marks, each an ordinary record
    - type: github
      href: https://github.com/acme/product
  legal:                       # the centered row above the copyright line
    - title: Privacy
      href: https://acme.com/privacy
  copyright: © 2026 Acme Inc.  # omit (or leave blank) for the built-in attribution line
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

A locale's tree is a sparse overlay on the site's effective default language, resolved per field
rather than per file. Every key it leaves out is inherited, and only the keys it writes are its
own, so a German tree that translates two section titles is two sections long and takes everything
else from English.

Membership merges by identity, not by replacement. Nodes are matched across languages by identity
alone, never by position: `page:` for a page, `href:` for a link, `id:` for a section, `type:` for
a social mark. Matching never crosses sections, and a node whose parent found no counterpart has
none either. A duplicated identity on either side makes both ambiguous, and they inherit nothing.
A list the locale does not mention inherits whole, and a locale that mentions some of a list still
inherits every entry it did not name - leaving a page out of a locale tree no longer excludes it.
One consequence is worth stating outright: a locale cannot point a link at a localized URL, because
changing `href` changes which node it is.

A locale may take ownership of one list's whole membership instead of just inheriting it, with a
boolean key written beside the list it governs: `ownToc` and `ownHeader` at the tree root, `ownNav`
inside `footer:`, and `ownChildren` beside the `children` of a section or a page entry - an external
link has no children, so it carries no such key. Absent or `false` leaves that list's membership
inherited, as above; `true` makes the merged list exactly the entries this file writes, in this
file's own order, so a default-language entry the file does not name is not in the list at all.

Detaching a list changes its membership and order and nothing else. An entry the file does name
inside a detached list still matches its default-language counterpart by identity and still
inherits every key it does not write itself. Detaching does not reach downward: a section or a page
with children of its own, sitting inside a detached list, keeps its own children inherited until it
carries its own `ownChildren: true`.

An empty list is still an inherited one on its own - writing an empty `toc:` with no `ownToc: true`
beside it inherits every entry the default language's table of contents has. Only the detach key
next to it makes the list genuinely empty.

`ownSocial` and `ownLegal` are reserved for `footer.social` and `footer.legal` and are not accepted;
those two stay identity-merged only, the same way described below.

An entry the locale does not want announced carries `hidden: true` there; the page still builds and
stays reachable at its own URL, but is left out of navigation, search, and every generated index for
that locale. An entry that is this language's own rather than a translation of anything carries
`inherited: false` instead - every item kind accepts it, it defaults to true, and it is a warning in
the default-language tree, where every item is already that language's own. An entry that claims a
translation and finds no counterpart is a warning, and `inherited: false` is how you answer it.

Hiding and dropping are different outcomes. `hidden: true` still builds the page; dropping happens
when detaching a list stops naming a page there and no other list of that locale - `header`, `toc`,
or `footer.nav` - names it either, so the page is not built for that locale at all. A page file the
locale had already translated that its merged tree no longer names anywhere becomes an orphan
(NAV_002).

A locale-only entry - one with no counterpart in the default language - is placed immediately after
the nearest preceding entry it shares the file with; write one bare identity line above it
(`- page: intro`) to anchor it in the middle of a list. Order otherwise always follows the
default-language tree: a locale that writes two matched entries in a contradicting order gets a
warning and the base order, because the written order cannot be honored without silently discarding
what the locale left unwritten.

A locale tree may take ownership of its own order instead, with one root key:

```yaml
ownOrder: true
```

Illegal in the default-language tree. While present, this file's order - across `toc`, `header`,
`footer.nav`, `footer.social` and `footer.legal`, and every section's `children` - is the order, and
a base entry this file does not name is placed after its preceding named entry, or before its
following named one, or at the end when neither is named. Membership still inherits either way;
`ownOrder` only detaches order. The tool window writes this key itself the first time a drag, a
sort, or a locale-only insertion needs it - a hand-written `ownOrder: true` means exactly what it
says.

`footer:` merges `social` and `legal` by identity through the same walk as `toc`/`header`, so a
locale may translate one mark's tooltip while every other mark, and every legal link, still
inherits. `copyright` stays a plain scalar override.

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
  resolve to `pages/guides/api.md`. Optional keys: `title` (label override), `home` (site root
  marker), `hidden` (announced nowhere, still built and reachable), `children` (nested items), and
  `inherited: false` (this entry is this language's own, not a translation).
- **External link**: `href:` with `title:`. A header link may add `type:` to draw it as a brand
  mark (one of the recognized brand names) or as a CTA: `primary-button` for the filled button,
  `secondary-button` for the outlined one, the same two names an inline
  `[Text](url){button="primary"}` uses on a page; omitting `type:` draws a plain row. `color:` goes
  with a button `type:` and states your own shade where the theme's accent would stand, so it is the
  fill of a primary and the border of a secondary.
- **Section**: `section:` with no value of its own, plus the siblings `id:`, `title:` and
  `children:`; sections group pages in the `toc` and may nest. Optional `collapsed: true`. The `id`
  is the section's stable identity and must be unique in the file; the `title` is what readers see,
  so renaming a section leaves every other file pointing at it alone.
- **Social mark** (`footer.social`): a record of `type:` (the brand, its identity), `href:`, and an
  optional `tooltip:` (the accessible name; falls back to the brand's own name). A locale mark can
  state only `tooltip:` and inherit `href:` from its default-language counterpart.
- **Legal link** (`footer.legal`): a record of `title:`, `href:` (its identity) and an optional
  `image:` (a project asset path drawn as a small `<img>` beside the label).

## Rules validation enforces

- The default-language tree must declare `title`, `header`, `toc` and `footer`. It inherits from
  nothing, so it has to be complete; a locale tree may omit all four, which simply means the whole
  tree is inherited.
- `inherited` is a WARNING in the default-language tree. In a locale tree, an entry with no
  counterpart in the default language is a WARNING until `inherited: false` says it is this
  language's own.
- `ownOrder` is a WARNING in the default-language tree, where there is nothing to detach from.
- `ownToc`, `ownHeader`, `ownNav`, and `ownChildren` are each a WARNING in the default-language tree
  too (NAV_025), for the same reason.
- A detach key written without the list it governs in the same file is a WARNING (NAV_026): with no
  list beside it the key detaches nothing, so that list stays inherited and the built site is unchanged.
- Two sections in one file may not share an `id`.
- Pair trees are required only for declared site memberships. A tree or sidecar for an undeclared
  pair is an ERROR and ignored; a locale directory absent from the root catalog is an orphan-locale
  ERROR and ignored.
- The root `title` must be non-empty.
- Only one page carries `home: true`.
- `header` is a flat list and `footer.nav` a flat list: no nested sections in either.
- `footer` is a map of `nav`, `social`, `legal` and `copyright`, not a list. The order between those
  four belongs to the theme, so no authored order can put a brand mark above a nav row; the order
  inside a slot is yours, or the default language's until `ownOrder: true` takes it back.
- An item is one thing: `page`, `section`, or `href`, never a combination.
