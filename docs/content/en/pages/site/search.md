---
title: Site Search
---

Every built site ships with search out of the box: the magnifier in the header opens a
query field, and results appear in a dropdown as you type. There is nothing to sign up for
and nothing to run: the index is a static file built with the site, and the search executes
entirely in the reader's browser.

## What gets found

The index covers every published page:

- **Titles**, with double weight: a page whose title matches ranks above a page that only
  mentions the term in passing.
- **Body text**, as readable prose: excerpts in the dropdown come from the actual page
  content, with the matched words highlighted.
- **Tags** from frontmatter: a query matching a tag finds the page even when the word never
  appears in the text.
- **Landing page sections**: badges, headings, and card titles of
  [landing pages](../writing/landing-pages.md) are searchable like any other content.

Matching is case-insensitive and prefix-aware: typing `deploy` also finds `deployment`.
The dropdown shows the top ten results.

## Keeping a page out of search

Set `hidden: true` in a page's frontmatter to publish it but keep it out of the search
index - useful for pages meant to be reached by a direct link only. Unpublished pages
(status other than `final`) are never indexed.

## Scale

The index is a single JSON file containing the site's text, downloaded once when the
reader first searches. This is a deliberate simplicity trade-off that serves documentation
sites comfortably into the hundreds of pages; the search itself adds no load anywhere but
the reader's browser tab.
