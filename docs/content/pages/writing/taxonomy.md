---
title: Tags, Categories, and Labels
---

Three ways to classify content, each with its own job: tags and categories are reader-facing
and ship with the page; labels are your internal workflow markers.

## Tags

Free-form keywords on a page:

```yaml
---
title: Reverse Proxy Setup
tags: [nginx, deployment]
---
```

Tags render in the page's meta row on the published site and are searchable: a query
matching a tag finds the page even when the word never appears in the text.

## Categories

Coarser grouping, also in frontmatter:

```yaml
categories: [Administration]
```

Categories render in the page's meta row alongside tags. Use categories for the few broad
areas your content falls into and tags for specific topics; a page typically has one
category and several tags.

## Labels

Labels live in `commons/labels.yml` (an id, a display name, and a color) and are managed in
the "Catalog" tool window together with the tag and category inventory. Labels are for the
writing workflow - marking pages as needs-review, outdated, translated - and stay in the
IDE: they are never published to the site.

The "Catalog" tool window is also where you see all tags and categories in one place, so
naming stays consistent instead of drifting into near-duplicates like `deploy` next to
`deployment`.
