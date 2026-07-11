---
title: Pages and Frontmatter
---

A page is a Markdown file under `content/pages/` with a YAML frontmatter block on top. The
body is regular Markdown: headings, lists, tables, task lists, fenced code with syntax
highlighting, strikethrough, and autolinked URLs.

```markdown
---
title: Installation
status: draft
tags: [setup]
---

## Requirements

...
```

## Frontmatter keys

- `title` - the page name shown in the navigation, the browser tab, and search results.
  Without it, the first `#` heading of the body is used.
- `status` - the publication state: `todo`, `draft`, `review`, or `final`. Anything other
  than `final` keeps the page OUT of the built site, its navigation, sitemap, and search.
  A page with no `status` counts as final.
- `tags` and `categories` - lists rendered in the page's meta row; see
  [Tags, Categories, and Labels](taxonomy.md).
- `date` - shown as the "Last modified" stamp; on blog posts it also orders the listing.
- `seo-description` - the meta description for this page (search engines and link previews).
- `hidden: true` - keeps the page out of the site search index while still publishing it.
  Useful for pages reached by a direct link only.
- `noindex: true` - emits a robots noindex meta tag for this page.
- `layout: full` - a landing page: the body mixes prose with `:::hero`, `:::features`,
  and other full-width sections. See [Landing Pages](landing-pages.md).

## Linking between pages

Link to another page by its file path, relative to the current file:

```markdown
See [Navigation](navigation.md) and [Project Structure](../creating/project-structure.md).
```

The build resolves these to the final site URLs, and validation flags a link whose target
does not exist. Images work the same way; files land in the site's assets automatically.

## Drafts in practice

Set `status: draft` while a page is in progress: you keep the live preview and validation,
but builds, deploys, and the PDF export skip the page. Flip to `final` (or remove the key)
to publish.
