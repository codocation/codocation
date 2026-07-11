---
title: Navigation
---

The navigation tree defines what the site shows and in what order. It lives in
`content/<site id>.tree.yml`, and the "Codocation" tool window is its visual editor: what
you see in the tree is what the published sidebar shows.

## Editing the tree

- **Add** pages with the tree's toolbar or context menu; the Markdown file is created along
  with the tree entry.
- **Reorder and nest** by drag-and-drop.
- **Group** pages into named sections; a section is a heading in the sidebar, not a page.
- **Set the home page**: the page marked as home is served at the site root. A docs site
  must have one (or a top-level `index.md`), or the build refuses to run.

## The three areas

The tree file has three parts, and the tool window edits all of them:

- `toc` - the table of contents: the sidebar of a docs site.
- `header` - links shown in the site header.
- `footer` - links shown in the site footer.

## What publishing follows

Build, deploy, and the PDF export follow the tree: pages appear in tree order, sections
become structure, and a Markdown file that no tree references is not published at all. A
page whose `status` is not final is skipped even when the tree references it; the sidebar,
sitemap, and search skip it with the page.

## Editing documentation settings

The tool window's gear menu has "Edit Documentation...", which edits the site's Title, ID,
and Description:

- **Title** is the site name shown in the header, footer, and browser tab.
- **ID** is the technical identifier from creation time. Changing it renames the
  documentation: the `sites.<id>` entry in `codocation.yml` moves to the new key, and the
  per-site files on disk (`<id>.tree.yml`, and `<id>.seo.yml` / `<id>.redirects.yml` when
  they exist) are renamed to match. It must stay a unique, normalized identifier (lowercase
  letters, digits, dashes) and cannot collide with another documentation's id.
- **Description** is a one-line summary shown in the site footer.

The site's Public URL is set elsewhere: see [Titles, URLs, and Branding](../site/branding.md).
