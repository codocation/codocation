---
title: Tags, Categories, and Labels
label: new
categories:
  - beta-testing
  - cloud_services
---

Three ways to classify content, each with its own job. Tags are free-form page taxonomy; labels and
categories are defined, reader-facing classifiers rendered in preview and published output.

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

## Categories {#taxonomy-categories label="new" categories="beta-testing, cloud_services"}

Categories are additive: a page can have several, in authored order. Documents store stable IDs,
not localized display names:

```yaml
---
label: new
categories:
  - beta-testing
  - cloud_services
---
```

The definition files use keyed identities. `definitions/categories.yml` owns optional global color;
`content/<locale>/definitions/categories.yml` owns localized `name` and optional `tooltip`.
Categories render below the page title or heading. Use them for broad areas and tags for specific
topics; a page may have one label and several categories.

## Labels

Root `definitions/labels.yml` owns keyed IDs and global `color`; locale files own localized `name`,
optional `compact`, and optional `tooltip`. IDs match `[a-z0-9_-]+` and are referenced from
frontmatter or heading attributes. Labels render above the page title or heading.

```yaml
# definitions/labels.yml
labels:
  new:
    color: "#10B981"

# content/en/definitions/labels.yml
labels:
  new:
    name: New
    compact: NEW
    tooltip: Recently added
```

## Heading assignments

Any H1-H6 heading accepts a terminal attribute block. Use a scalar `label` and a typed,
comma-separated `categories` list; formatting is canonical comma-space:

```markdown
## Billing {#billing label="new" categories="beta-testing, cloud_services"}
```

`{#billing}` is the explicit anchor. Anchors must be unique in the rendered page; duplicate
anchors are errors with rename/remove quick fixes. Missing IDs are errors with a create-definition
quick fix. Repeated labels or category IDs warn and keep the first occurrence.

## Catalog workflow

Catalog creation asks for localized `Name` and tooltip, an editable global `ID`, optional global
`Color`, and label-only compact text. IDs are suggested by Unicode transliteration and normalized
to `[a-z0-9_-]+`, but remain editable before confirmation. Creating from a selected locale writes
the root identity and that locale payload atomically. Editing an ID renames all locale keys and
references; deleting can remove only the selected locale or the entity everywhere. Removing the
last assignment never deletes definition data.
