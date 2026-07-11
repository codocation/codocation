---
title: Docs and Blog
---

A site is either a docs site or a blog. The type is chosen when the documentation is
created and stored per site in `codocation.yml` (`type: docs` or `type: blog`).

## Docs

A docs site is driven by its navigation tree: the sidebar mirrors the tree, pages appear in
tree order, and the page marked as home is served at the site root. Everything in the
[Writing](../writing/pages.md) chapter applies as is.

## Blog

A blog is driven by dates instead of a tree. Posts are regular Markdown pages under
`content/pages/posts/`, and the listing page contains the `{{posts}}` placeholder where the
post cards render:

```markdown
---
title: Blog
---

# Blog

{{posts}}
```

- **Ordering**: newest first, by the `date` frontmatter key.
- **Pagination**: ten posts per page; older posts move to `/page/2/` and beyond
  automatically.
- **Cards**: each card shows the post title, date, an excerpt, and a cover image. The
  excerpt is the beginning of the post (up to the first thematic break `---`, capped at 300
  words); the cover is the `cover` frontmatter image, or the first image in the post.

## Post statuses

The `status` frontmatter key drives publication for docs pages and posts alike: `todo`,
`draft`, and `review` keep the page out of the built site; `final` (or no status) publishes
it. On a blog this doubles as the writing pipeline: keep a post in `draft` while writing,
move it to `review`, and flip to `final` to ship - the listing, sitemap, and search follow
automatically.

## Docs and blog side by side

One project can host both: create a second site (see
[Existing Project](../creating/existing-project.md)) and give each site its own `basePath`
in `codocation.yml` so they mount at different URLs, for example the docs at the root and
the blog under `/blog/`.
