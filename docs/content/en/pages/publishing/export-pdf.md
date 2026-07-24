---
title: Export a PDF
---

The same content that builds your site can ship as one polished PDF: a title page, a table of
contents, and every published page in navigation order.

## Run an export

Choose `Tools → Codocation → Export PDF`, pick where to save the file, and wait for the
background task to finish. The notification links to the exported file.

## PDF settings

The "PDF" tab of the "Site & Export" tool window configures the selected site and requested
locale. PDF settings resolve per leaf through this chain:

`requested locale → sites.<siteId>.defaultLocale → sites.<siteId>.pdf → global pdf → built-in defaults`

Locale overrides live in `content/<locale>/<siteId>.pdf.yml` and contain PDF leaves directly,
without a wrapping `pdf:` key:

```yaml
titlePage:
  enabled: true
  title: Codocation Documentation
  subtitle: Author and publish documentation
  logo: images/pdf/logo.svg
pageNumbers:
  enabled: true
  format: "{page} of {total}"
```

The merge is presence-aware. A leaf absent from a sidecar remains available from lower layers;
a present leaf replaces that leaf wholesale, with no deep or list merging. A locale PDF sidecar
may override technical leaves too, including orientation, table of contents, page numbers, or
the selected global PDF font. Editing the Locale scope writes only the requested locale and
never changes a fallback source.

- `titlePage` adds a cover; its title, subtitle, and locale-owned PDF logo are configurable in
  the "PDF" tab. `titlePage.title` and `titlePage.subtitle` localize cover text.
- `font` selects a global `ttf` or `otf` file under `assets/fonts/` for PDF embedding. A
  locale may select a different global file, but the binary itself is never locale-owned. A
  missing file, an invalid font format, or a font the PDF renderer cannot read falls back to
  the system font instead of failing the export. This is separate from the site's web font,
  which also accepts `woff2` and `woff`, so the two can be set independently.
- `orientation` is `portrait` or `landscape`.
- `tableOfContents` inserts a clickable contents section after the cover.
- `pageNumbers` stamps pages when enabled; `format` supports `{page}` and `{total}`, and
  `position` is `bottom-left`, `bottom-center`, or `bottom-right`.

## What gets included

The export first selects a site, then a locale declared for that site. It follows that requested
locale's navigation tree and includes published pages only:
drafts and pages hidden from navigation stay out, the same way they stay off the built site.
An explicitly fallback-marked page may render the site's `defaultLocale` source, while all internal
links retain the requested locale and apply each target page's own translation state.
