---
title: Project Structure
---

A Codocation project is plain files: YAML configuration, locale-owned Markdown, and explicit
asset and definition namespaces. Everything is readable, diffable, and belongs in version
control.

```
codocation.yml            project configuration
error-registry.yml        project-wide diagnostic severity overrides
assets/
  media/                  non-localized browser/chrome media, including the favicon
  fonts/                  global font binaries
  css/                    global site CSS
  js/                     global site JavaScript and analytics
definitions/               invariant definition IDs and fields
content/                    authored and published content root
  <locale>/                 one language variant, such as en/
    <site id>.tree.yml       required title and navigation tree
    <site id>.pdf.yml        optional locale PDF overrides
    <site id>.seo.yml        optional locale SEO overrides
    <site id>.redirects.yml  optional locale redirects
    pages/                   the locale's Markdown pages
      index.md
      getting-started.md
      posts/                 blog posts, when the site is a blog
    images/                  locale-owned page, branding, and PDF images
    attachments/             locale-owned downloadable files
    definitions/             translated definition payloads
```

The root `definitions/` directory includes keyed `labels.yml` and `categories.yml` files. Their
locale-owned counterparts contain localized names, optional label compact text, and tooltips.
Each locale may omit an identity (unavailable) or contain the explicit `translation: fallback`
entry to use the complete default payload; optional fields omitted from a local entry remain absent
in that locale.

## codocation.yml

The single technical project configuration file: site ids and types, URL and web settings,
build output settings, the configured locales, and the deploy target. Reader-facing title and
description, localized PDF text, SEO meta tags, and redirects live in each locale's site files.

## The navigation tree

`content/<locale>/<site id>.tree.yml` owns the required site title, optional description, and
what that locale's site shows and in what order: the header links, the table of contents (with
nested sections and the home-page marker), and the footer links. Each configured locale has an
independent tree; there is no implicit merge between locales. The "Codocation" tool window
edits it visually; see [Navigation](../writing/navigation.md).

## Pages

Markdown files under `content/<locale>/pages/`, each with a small frontmatter block. A page not
referenced by any tree is simply not published. Blog posts live under
`content/<locale>/pages/posts/`.

The physical file `content/en/pages/guide.md` is referenced by its logical tree path
`pages/guide.md`; the physical `content/<locale>/` prefix is never part of a tree reference.
The language variants themselves are listed under the `locales:` registry in `codocation.yml`.

## Images, attachments, and assets

`content/<locale>/images/` holds locale-owned pictures referenced from pages, along with
locale-aware branding and PDF images. `content/<locale>/attachments/` holds downloadable files
linked from pages. The favicon is non-localized at `assets/media/`; fonts, CSS, JavaScript, and
analytics are global under their corresponding `assets/` subspaces. A build copies resolved
resources into the matching output directory; see [Build the Site](../publishing/build-site.md).
Images and attachments follow the page's source provenance: a fallback page displayed in a
requested locale uses the source locale's media first, then its permitted fallback.

## Definitions

Root `definitions/` stores stable IDs and invariant fields. Locale-owned
`content/<locale>/definitions/` stores translated payloads, with sparse non-default locales
falling back by stable ID to the configured default locale. They are edited in the
"Definitions" and "Catalog" tool windows; see [Variables](../writing/variables.md) and
[Definitions](../writing/definitions.md).
