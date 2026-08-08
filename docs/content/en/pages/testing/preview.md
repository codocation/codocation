---
title: Live Preview
---

The preview shows the page being edited exactly as the published site will render it: the same
Markdown pipeline, theme, and styles that the build ships.

## Locale and provenance

Preview resolves the requested locale first. A page with no file in that locale comes from the
site's `defaultLocale` without a physical copy and without a diagnostic. The resolved state retains
`requestedLocale`, `sourceLocale`, `logicalPath`, and `physicalPath`; internal links keep the
requested locale even when the current page came from the default one. Images and attachments follow
the page's source provenance too.

After a valid preview, the IDE can keep showing its last-good page while reporting a new ERROR. On
an initially invalid project, the TOC keeps a broken node and preview says so rather than rendering
a guess.

## How it works

Open a page from the "Codocation" tool window and the "Preview" tool window follows: it
renders the active editor's page and updates live as edits happen, without saving. Scrolling is
synchronized, so the preview keeps pace with the place being edited. Clicking a link, whether
in the left-side navigation or in the page body, browses to that page inside the preview and
resolves the target against its own files.

## Follow and Detached modes

- **Follow** (the default) keeps the preview linked to the editor in both directions. Clicking
  a link in the preview opens that page in the editor, and opening or switching a file in the
  editor updates the preview. The editor and preview always show the same page.
- **Detached** frees the preview from the editor. Clicking a link browses the preview on its
  own without changing the editor, and the editor stays where it is. An edit in the editor
  still reaches the preview when the edited page is the one currently shown there.

The header icon turns orange while Detached is active, as a reminder that the preview is
browsing independently.

## Open in browser

The preview can also be opened in a regular browser tab, where it mirrors the IDE preview: it
follows the same page, picks up edits live without a manual reload, and a link clicked in the
browser drives the IDE preview in turn. The mode toggle is not shown in the browser tab since
the mode is driven from the IDE. It uses the same requested-locale resolution and provenance
rules.

## Themes

The preview renders in a light or dark site theme, so both looks can be checked before
publishing. Code highlighting follows the theme, and the reader's theme choice is persisted.

## What the preview does not show

Cross-page build artifacts such as pagination, the search index, and the sitemap require a real
build. Run [Build the site](../publishing/build-site.md) to inspect the complete output.
