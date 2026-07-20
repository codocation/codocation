---
title: Tags, Categories, and Labels
---

Three ways to classify content, each with its own job: tags and categories are reader-facing
and ship with the page; labels are internal workflow markers. Categories are derived from page
frontmatter; there is no categories definition file.

## Tags

Free-form keywords on a page:

```yaml
---
title: Reverse Proxy Setup
tags: [nginx, deployment]
---
```

Tags render in the page's meta row on the published site and are searchable: a query matching a
tag finds the page even when the word never appears in the text.

## Categories

Coarser grouping, also in frontmatter:

```yaml
categories: [Administration]
```

Categories render in the page's meta row alongside tags. Use categories for the few broad areas
your content falls into and tags for specific topics; a page typically has one category and
several tags.

## Labels

Labels use the normalized definition contract. Root `definitions/labels.yml` contains each
label's `id` and `color`; `content/<locale>/definitions/labels.yml` contains the matching `id`,
display `name`, optional `compact` text, and optional `tooltip`. A locale cannot introduce an
unknown label or change its invariant color. Sparse locale payloads fall back by stable ID to
the default locale. Labels are managed in the "Catalog" tool window and stay in the IDE; they
are never published to the site.

The "Catalog" tool window also shows tags and categories in one place, so naming stays
consistent instead of drifting into near-duplicates like `deploy` next to `deployment`.
