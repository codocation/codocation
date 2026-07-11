---
title: Live Preview
---

The preview shows the page being edited exactly as the published site will render it:
the same Markdown pipeline, the same theme, the same styles that the build ships.

## How it works

Open a page from the "Codocation" tool window and the "Preview" tool window follows: it
renders the active editor's page and updates live as edits happen, without saving. Scrolling is
synchronized, so the preview keeps pace with the place being edited.

The preview is multi-page: clicking a link, whether in the left-side navigation or in the page
body, browses to that page inside the preview.

Because the preview and the build share one rendering path, what shows in the preview is
what readers get: resolved links, substituted [variables](../writing/variables.md), images,
code highlighting, and the site chrome around the article.

## Follow and Detached modes

A toggle in the preview's header switches between two modes:

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
browser drives the IDE preview in turn. The mode toggle is not shown in the browser tab, since
the mode is driven from the IDE.

## Themes

The preview renders in a light or dark site theme, so both looks can be checked before
publishing. Code highlighting follows the theme.

## What the preview does not show

Cross-page artifacts exist only in a real build: the blog listing's pagination, the search
index, the sitemap. For the complete result, run
[Build the site](../publishing/build-site.md) and open the output in a browser.
