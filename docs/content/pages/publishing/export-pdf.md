---
title: Export a PDF
---

The same content that builds your site can ship as one polished PDF: a title page, a table of
contents, and every published page in navigation order.

## Run an export

Choose `Tools → Codocation → Export PDF`, pick where to save the file, and wait for the
background task to finish. The notification links to the exported file.

## PDF settings

The "PDF" tab of the "Site & Export" tool window configures the document. The same values
live under `pdf:` in `codocation.yml`:

```yaml
pdf:
  titlePage:
    enabled: true
  font: assets/fonts/Inter-Regular.ttf
  orientation: portrait
  tableOfContents: true
  pageNumbers:
    enabled: true
    format: "{page} of {total}"
    position: bottom-center
```

- `titlePage` adds a cover; its title, subtitle, and logo are configurable in the "PDF" tab.
- `font` embeds a font file into the PDF, picked in the "PDF" tab. It takes a `ttf` or `otf`
  file under `content/assets/fonts/` - a `woff2` or `woff` file, a missing file, or a file the
  PDF renderer cannot read falls back to the system font instead of failing the export. This
  is a separate setting from the site's own web font (which also accepts `woff2`/`woff`), so
  the two can be set independently.
- `orientation` is `portrait` or `landscape`.
- `tableOfContents` inserts a clickable contents section after the cover.
- `pageNumbers` stamps each page; `format` supports the `{page}` and `{total}` placeholders,
  and `position` is `bottom-left`, `bottom-center`, or `bottom-right`.

## What gets included

The export follows the navigation tree and includes published pages only: drafts and pages
hidden from the navigation stay out, the same way they stay off the built site.
