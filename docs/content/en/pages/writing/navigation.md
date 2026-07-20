---
title: Navigation
---

The navigation tree defines what the site shows and in what order. It lives in
`content/<locale>/<siteId>.tree.yml`, and the "Codocation" tool window is its visual editor.
Every configured locale has its own required tree; navigation never falls back.

## Editing the tree

- **Add** pages with the tree's toolbar or context menu; the Markdown file is created along
  with the tree entry.
- **Reorder and nest** by drag-and-drop.
- **Group** pages into named sections.
- **Set the home page** with `home: true`; a docs site must have one, or a top-level
  `pages/index.md`, or the build refuses to run.
- **Mark a translation fallback** with `translation: fallback` in a non-default locale. This
  is explicit and resolves the default-locale source without copying it. A page omitted from
  the locale tree is excluded; there is no exclusion marker.

## The three areas

The tree file has three parts, and the tool window edits all of them:

- `toc` - the table of contents and sidebar.
- `header` - links shown in the site header.
- `footer` - links shown in the site footer.

Tree page references use canonical logical paths such as `pages/getting-started.md`, not a
physical locale path.

## What publishing follows

Build, deploy, and PDF export follow the requested locale's tree. A page whose `status` is not
final is skipped even when referenced. A page marked `translated` must exist in the requested
locale; a missing file is an ERROR and is not replaced with default content. A non-default
page explicitly marked `fallback` resolves the default-locale source. The resolved page keeps
requested/source provenance, and its links still resolve in the requested locale.

## Editing documentation settings

The tool window's gear menu has "Edit Documentation...", which edits the site's title, ID, and
description:

- **Title** is stored in `content/<locale>/<siteId>.tree.yml`.
- **ID** renames the `sites.<id>` entry and every configured locale's matching tree and
  optional sidecars (`<id>.pdf.yml`, `<id>.seo.yml`, and `<id>.redirects.yml`).
- **Description** is stored next to `title` in the tree file.

The site's Public URL is set elsewhere: see [Titles, URLs, and Branding](../site/branding.md).
