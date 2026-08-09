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
- **Translate a title** by writing only that key in the other language's tree. Everything you leave
  out is inherited from the default language, so a translated tree is usually a few lines long.
- **Mark an entry as this language's own** with `inherited: false`, for something that translates
  nothing in the default language. Without it, such an entry is reported as having no counterpart.
- **Hide an entry** with `hidden: true`. The page still builds and stays reachable at its own URL,
  but it is announced nowhere: not in navigation, search results, the tag and category archives,
  `llms.txt`, or `sitemap.xml`.

## What a language's tree inherits

A locale tree is a sparse overlay on the default language's, resolved key by key rather than file
by file. Every key it leaves out is inherited; only the keys it writes are its own.

Membership is inherited the same way, so **there is no way to shorten a list by omission**. A list
the language does not mention inherits whole, and a list it does mention still inherits every entry
it did not name. Writing `hidden: true` on the entry is how one language drops a page from its
navigation, and the entry stays in the file where it can be found again.

Entries are matched across languages by identity alone, never by position: `page:` for a page,
`href:` for a link, `id:` for a section, `type:` for a social mark. Matching never crosses sections,
and an entry whose parent found no counterpart has none either. One consequence is worth stating
outright: a language cannot point a link at a localized URL, because changing `href` changes which
entry it is.

Order follows the default language. A locale-only entry is placed after the nearest preceding entry
it shares the file with, and two matched entries written in a contradicting order get a warning and
the default order - the written order cannot be honored without silently dropping the entries the
file never mentioned. A tree that wants its own order says so with one root key, `ownOrder: true`,
which is illegal in the default language; the tool window writes it itself the first time a drag or
a sort needs it.

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
