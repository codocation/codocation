---
title: Navigation
---

The navigation tree defines what the site shows and in what order. It lives in
`content/<locale>/<siteId>.tree.yml`, and the "Codocation" tool window is its visual editor.
A site's default-language tree is required and must be complete, because it inherits from nothing.
Every other language's tree is optional: it is a sparse overlay, and a language with no tree file at
all simply shows the default language's navigation.

## Editing the tree

- **Add** pages with the tree's toolbar or context menu; the Markdown file is created along
  with the tree entry.
- **Reorder and nest** by drag-and-drop.
- **Group** pages into named sections.
- **Set the home page** with `home: true`; a docs site must have one, or a top-level
  `pages/index.md`, or the build refuses to run.
- **Translate a label** by writing only that key in the other language's tree. Everything you leave
  out is inherited from the default language, so a translated tree is usually a few lines long.
- **Mark an entry as this language's own** with `inherited: false`, for something that translates
  nothing in the default language. Without it, such an entry is reported as having no counterpart.

Omitting a page from a language's tree does not exclude it, because a list that language does not
write is inherited whole. To exclude a page from one language, write the list without it.

## The three areas

The tree file has three parts, and the tool window edits all of them:

- `toc` - the table of contents and sidebar.
- `header` - a single ordered list of what the site header shows: plain links, brand marks, and CTA
  buttons, in the order you write them.
- `footer` - a map of the four parts the theme paints: `nav`, `social`, `legal` and `copyright`. The
  order between those four is the theme's; the order inside each is yours.

Tree page references with no ancestor page, whether at the root or inside intervening sections, use
canonical logical paths such as `pages/getting-started.md`; exactly one bare Markdown filename such
as `getting-started.md` is also accepted as a fallback. Any descendant of a page resolves relative
to the nearest ancestor page's directory, even when sections intervene. For example,
`pages/reference.md` may have a child `reference/api.md`, which resolves to
`pages/reference/api.md`. These are logical paths, not physical locale paths.

## What publishing follows

Build, deploy, and PDF export follow the requested language's effective tree, which is that
language's own file resolved over the default language's. A page whose `status` is not final is
skipped even when referenced.

Where a page's text comes from is not declared in the tree at all. It follows the file: the
requested language's own page under `pages/` when that file exists, and the site's effective default
language when it does not. A page with no file in the requested language is not a diagnostic, since
that is the ordinary state of an untranslated page. The resolved page keeps requested and source
provenance, and its links still resolve in the requested language.

## Editing documentation settings

The tool window's gear menu has "Edit Documentation...", which edits the site's title, ID, and
description:

- **Title** is stored in `content/<locale>/<siteId>.tree.yml`.
- **ID** renames the `sites.<id>` entry and every declared membership's matching tree and
  optional sidecars (`<id>.pdf.yml`, `<id>.seo.yml`, and `<id>.redirects.yml`).
- **Description** is stored next to `title` in the tree file.

The site's Public URL is set elsewhere: see [Titles, URLs, and Branding](../site/branding.md).
