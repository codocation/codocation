---
title: Project Structure
---

A Codocation project is plain files: YAML configuration and Markdown content. Everything is
readable, diffable, and belongs in version control.

```
codocation.yml            project configuration
content/
  <site id>.tree.yml      navigation tree of a site
  pages/                  the pages (Markdown)
    index.md
    getting-started.md
    posts/                blog posts, when the site is a blog
  images/                 page images, logo, favicon
  attachments/            downloadable files referenced from pages
  assets/                 custom CSS, custom JS, the analytics script
    fonts/                custom fonts
commons/
  variables.yml           reusable values for {{name}} substitution
  glossary.yml            term definitions
  keymaps.yml             keyboard shortcut definitions
  labels.yml              labels for the catalog
```

## codocation.yml

The single configuration file: the sites map (title, type, base URL, description per site),
build output settings, PDF settings, and the deploy target. The "Site & Export" tool window
and `Settings → Tools → Codocation` edit it for you, and it is always safe to edit by hand.

## The navigation tree

`content/<site id>.tree.yml` lists what the site shows and in what order: the header links,
the table of contents (with nested sections and the home-page marker), and the footer links.
The "Codocation" tool window edits it visually; see
[Navigation](../writing/navigation.md).

## Pages

Markdown files under `content/pages/`, each with a small frontmatter block. A page not
referenced by any tree is simply not published. Blog posts live under
`content/pages/posts/`.

## Images, attachments, and assets

`content/images/` holds pictures referenced from pages, along with the site logo and favicon.
`content/attachments/` holds downloadable files linked from pages. `content/assets/` holds
custom CSS, custom JS, and the analytics script, with fonts under `content/assets/fonts/`. A
build copies each of these directories into the matching directory of the output; see
[Build the Site](../publishing/build-site.md).

## Commons

Shared definitions that pages reuse: variables, glossary terms, keyboard shortcuts, labels.
They are edited in the "Definitions" and "Catalog" tool windows; see
[Variables](../writing/variables.md) and [Definitions](../writing/definitions.md).
