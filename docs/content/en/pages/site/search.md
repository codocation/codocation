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
- **Keywords** from frontmatter: words a page should be findable by without showing them.
  Unlike a tag, a keyword draws no chip, gets no archive page, and takes no URL segment.
- **Landing page sections**: badges, headings, and card titles of
  [landing pages](../writing/landing-pages.md) are searchable like any other content.

Matching is case-insensitive and prefix-aware: typing `deploy` also finds `deployment`.
The dropdown shows the top ten results.

## Keeping a page out of search

Write `hidden: true` on the page's entry in the navigation tree. The page is still built and
still reachable at its own URL, but it is announced nowhere: not in the sidebar, not in the
default search results, not in the tag and category archives, `llms.txt`, or `sitemap.xml`.
That is useful for a page meant to be reached by a direct link only. See
[Navigation](../writing/navigation.md).

`hidden` belongs to the tree entry, not to the page's frontmatter: one language may hide a page
the others announce, and a page has no place to say that.

Unpublished pages - any `status` other than `final` - are never built at all, so the question of
indexing them does not arise.

## Scale

The index is a single JSON file containing the site's text, downloaded once when the
reader first searches. This is a deliberate simplicity trade-off that serves documentation
sites comfortably into the hundreds of pages; the search itself adds no load anywhere but
the reader's browser tab.
