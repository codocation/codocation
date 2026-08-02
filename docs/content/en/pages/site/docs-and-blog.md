---
title: Docs and Blog
---

A site is either a docs site or a blog. The type is chosen when the documentation is created
and stored per site in `codocation.yml` (`type: docs` or `type: blog`).

## Docs

A docs site is driven by the requested locale's navigation tree: the sidebar mirrors that
tree, pages appear in tree order, and the page marked as home is served at the site root.
Everything in the [Writing](../writing/pages.md) chapter applies as is.

## Blog

A blog uses the required locale/site tree to decide which posts are included and what
translation state each post has. Dates order the posts that are already included by that
tree; they do not replace the tree. Posts are regular Markdown pages under
`content/<locale>/pages/posts/`, and the listing page carries the `{% posts %}` directive
where post cards render:

```markdown
---
title: Blog
---

# Blog

{% posts %}
```

- **Ordering**: newest first, by the `date` frontmatter key.
- **Pagination**: `web.listing.perPage` posts per page (ten by default); older posts move to
  `/page/2/` and beyond.
- **Empty state**: without posts the listing shows `No posts yet.`; a locale's
  `content/<locale>/site-strings.yml` can override that text with a `postsEmpty` entry, which is
  where a translated listing states it in its own language.
- **Cards**: each card shows the post title, date, an excerpt, and a cover image. The excerpt
  is the beginning of the post up to the first thematic break `---`, capped at 300 words; the
  cover is the `cover` frontmatter image, or the first image in the post.

## Post statuses

The `status` frontmatter key drives publication for docs pages and posts alike: `todo`,
`draft`, and `review` keep the page out of the built site; `final` (or no status) publishes
it. On a blog this doubles as the writing pipeline: keep a post in `draft` while writing,
move it to `review`, and flip to `final` to ship — the listing, sitemap, and search follow
automatically.

## Translation and publication

Each declared site membership has its own required tree; that tree controls which pages are
included and each page's translation state. A page is `translated` by default and must exist
physically in that locale; a non-default tree can explicitly mark it `translation: fallback` to
enable fallback to that site's `defaultLocale` without a copy. The marker does not choose the
source. Page images and attachments follow the page's source provenance. Draft, todo, and review
pages stay out of the built site; `final` or omitted status publishes them.

## Docs and blog side by side

One project can host both: create a second site (see
[Existing Project](../creating/existing-project.md)) and give each its own `basePath` in
`codocation.yml` so they mount at different URLs, for example the docs at the root and the
blog under `/blog/`.
