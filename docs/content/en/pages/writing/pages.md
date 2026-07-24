---
title: Pages and Frontmatter
---

A page is a Markdown file under `content/<locale>/pages/` with a YAML frontmatter block on
top. For example, the physical English file `content/en/pages/guide.md` has the canonical
logical path `pages/guide.md` in a tree; the physical locale prefix is not part of that path.
The catalog locale is named under the root `locales:` map; a site publishes it only when the code
appears in `sites.<siteId>.locales`.
The body is regular Markdown: headings, lists, tables, task lists, fenced
code with syntax highlighting, strikethrough, and autolinked URLs.

```markdown
---
title: Installation
status: draft
tags: [setup]
---

## Requirements

...
```

## Page title and metadata

The effective page title is `frontmatter.title` when present; otherwise it is the first H1. A
page with neither is an error. The title metadata bundle is `title`, `anchor`, `label`, and
`categories`, stored with the canonical title source:

```yaml
---
title: Payments
anchor: billing
label: new
categories:
  - beta-testing
  - cloud_services
---
```

An H1 can own the same bundle instead:

```markdown
# Payments {#billing label="new" categories="beta-testing, cloud_services"}
```

`anchor` (or `{#anchor}`) names a rendered page anchor; it is not a file or route identity. Every
explicit anchor must be unique in a page. H2-H6 attributes apply to those headings individually.
Frontmatter wins singular fields when both sources exist; categories recover as a stable distinct
union in frontmatter-first order. Codocation reports a split-bundle warning and offers atomic
quick fixes to keep the complete bundle in either frontmatter or the H1.

## Translation state

The supported states are `translated` and `fallback`:

- Omitting `translation` means `translated`, including in the site's effective default tree.
- `translation: translated` requires the page file in the requested locale. If it is absent,
  Codocation reports an ERROR and does not substitute default-locale content.
- `translation: fallback` enables fallback but does not choose the source. It is valid only in a
  non-default site locale and resolves the logical page from that site's `defaultLocale` without
  copying it. A fallback marker in the effective default tree, or beside an existing requested-
  locale file, is an ERROR. A missing source is a reader failure.
- Omitting the page from a locale tree excludes it from that locale. There is no
  `translation: excluded` state or `reason` field.

Every resolved resource keeps `requestedLocale`, `sourceLocale`, `logicalPath`, and
`physicalPath`. Internal links from an English fallback page displayed in German retain the
requested locale and apply the target page's own translation state. Writes always target the
requested locale, never the fallback source.

## Other frontmatter keys

- `title` - the page name shown in navigation, the browser tab, and search results. Without it,
  the first `#` heading of the body is used.
- `status` - the publication state: `todo`, `draft`, `review`, or `final`. Anything other
  than `final` keeps the page out of the built site, its navigation, sitemap, and search. A
  page with no `status` counts as final.
- `tags` and `categories` - tags are free-form; labels and categories resolve keyed IDs and render
  above/below the title or heading; see
  [Tags, Categories, and Labels](taxonomy.md).
- `date` - shown as the "Last modified" stamp; on blog posts it also orders the listing.
- `seo-description` - the meta description for this page (search engines and link previews).
- `hidden: true` - keeps the page out of the site search index while still publishing it. This
  is useful for pages reached by a direct link only.
- `noindex: true` - emits a robots noindex meta tag for this page.
- `layout: full` - a landing page: the body mixes prose with `:::hero`, `:::features`, and
  other full-width sections. See [Landing Pages](landing-pages.md).

## Canonical links

Relative Markdown links are normalized against one virtual logical root containing the four
allowed namespaces: `pages/`, `images/`, `attachments/`, and `assets/`. They are not resolved
against a physical locale directory.

```markdown
See [Navigation](navigation.md) and [Project Structure](../creating/project-structure.md).
![Diagram](../../images/content/layout.png)
[Download](../../attachments/manual.pdf)
[Styles](../../assets/css/site.css)
```

Fragments and queries are preserved. A path that climbs above the virtual root, starts with an
unknown namespace, or uses an unsupported `assets/` child is invalid. Missing images or
attachments are validation errors after the permitted source-aware lookup.

## Drafts in practice

Set `status: draft` while a page is in progress: preview and validation remain available, but
builds, deploys, and PDF export skip the page. Flip to `final` (or remove the key) to publish.
