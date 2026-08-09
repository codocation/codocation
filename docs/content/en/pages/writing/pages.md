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

## Which language's text a page shows

Nothing declares this. A page follows its own file: the requested language's page under
`content/<locale>/pages/` when that file exists, and the site's effective default language when it
does not. Translating a page means adding the file; there is no marker to write and none to keep in
step with it.

A page with no file in the requested language is not a diagnostic. That is the ordinary state of an
untranslated page, and of every project with one language.

The older `translation` key is rejected outright, along with its `translated`, `fallback` and
`excluded` states and the `reason` field beside it. What replaced part of it lives in the navigation
tree rather than in a page: `inherited: false` marks a tree entry as this language's own instead of a
translation of one in the default language. It says nothing about where any page's text comes from.
See [Navigation](navigation.md).

Every resolved resource keeps `requestedLocale`, `sourceLocale`, `logicalPath`, and `physicalPath`.
Internal links from an English page displayed in German retain the requested language and resolve
each target against its own files. Writes always target the requested language, never the source it
resolved from.

## Other frontmatter keys

These are all of them; anything else is reported as an unknown field (`MD_003`).

- `title` - the page name shown in navigation, the browser tab, and search results. Without it,
  the first `#` heading of the body is used.
- `status` - the publication state: `todo`, `draft`, `review`, or `final`. Anything other
  than `final` keeps the page out of the built site, its navigation, sitemap, and search. A
  page with no `status` counts as final.
- `tags`, `categories`, `label` - tags are free-form; labels and categories resolve keyed IDs and
  render above/below the title or heading; see [Tags, Categories, and Labels](taxonomy.md).
- `keywords` - words this page should be findable by in search without showing them: no chip, no
  archive page, no URL. Not a taxonomy; that is what `tags` is for.
- `description` - the machine-read summary: the `<meta name="description">` and what the AI exports
  carry. It is never shown on a card, and nothing else changes it.
- `summary` - the human annotation a card, a listing entry, and a link hover preview show. The
  `summary=` of the heading that renders as the page title wins over it; without either, a card and
  a hover fall back to the first prose paragraph and a listing entry to its own longer excerpt.
- `image` - the cover image the editor's metadata panel shows; `cover` - the cover image blog
  listings show.
- `author` - the author name shown on the page.
- `published` and `updated` - the two dates, each taking the same four forms: omit the key and the
  site type decides (a blog shows `published`, a docs site shows `updated`); `false`, `null`,
  `"none"` or `""` hide that date even where the site type would show it; `true` shows the
  automatically resolved value; any other string is shown verbatim and beats every automatic source.
  Automatically, `published` is the file's first commit day and `updated` its last, falling back to
  the file's modification time when it is dirty or untracked. When both resolve to the same day,
  only `published` renders.
- `order` - a numeric sort-order hint for navigation.
- `noindex: true` - emits a robots noindex meta tag for this page. `no_index` is an alias of it.
- `ai-export` - whether this published page reaches the AI-facing exports; omitting it follows the
  page's effective search indexability.
- `canonical` - the canonical URL to declare for this page, emitted as `<link rel="canonical">`.
- `layout: full` - a landing page: the body mixes prose with `:::hero`, `:::cards`, and
  other full-width sections. See [Landing Pages](landing-pages.md).
- `contributeUrl` - overrides this page's "Edit this page" link. A non-empty absolute
  http(s) URL replaces the link derived from the site's own `web.links.contributeUrl`;
  `false`, an empty string, a bare key, or a YAML null removes the link from this page. See
  [Titles, URLs, and Branding](../site/branding.md).

There is no `hidden` key here. Keeping a page out of navigation and every generated index is the
navigation tree's decision, not the page's: write `hidden: true` on the entry instead, and see
[Navigation](navigation.md).

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
