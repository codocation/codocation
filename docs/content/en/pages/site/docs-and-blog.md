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

A post is an ordinary page that lives under `content/<locale>/pages/posts/` and belongs in the
navigation tree like every other page - the scaffolded blog puts it in a section named after its
year. The RSS feed is configured with `web.feed`; it can carry the published, discoverable pages
of either site type. See [RSS Feeds](feeds.md) for its dates, stream meaning, and reader behavior.
The listing page carries the `{% posts %}` directive where post cards render:

```markdown
---
title: Blog
---

# Blog

{% posts %}
```

- **Ordering**: newest first, by the post's resolved `published` date - the value the frontmatter
  states, or the file's first commit day when it states none. See
  [Pages and Frontmatter](../writing/pages.md).
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
move it to `review`, and flip to `final` to ship - the listing, sitemap, and search follow
automatically.

`status` and `hidden` answer different questions. A non-`final` status means the page is not built:
no file, no URL, nothing to link to. `hidden: true` on a tree entry means the opposite - the page
is built and reachable at its own URL. If every entry naming it is hidden, the page is withheld
from discovery surfaces such as the sidebar, the prev/next strip, `sitemap.xml`, `llms.txt`, the
default search results, the feed, and the tag and category archives.

So an unlisted post - one you hand out by link and do not put on the front page - is `hidden: true`
on its entry, and a post that should not exist yet at all is `status: draft`.

## Translation and publication

A site's default-language tree controls which pages are included. Every other language overlays it
and inherits whatever it does not write, so membership is stated once and translated where it needs
to be. Which language's text a page shows is a separate question the tree does not answer: a page
resolves from the requested language's own file when that file exists, and from the site's
`defaultLocale` when it does not, with no marker and no diagnostic either way. Page images and
attachments follow the page's source provenance. Draft, todo, and review pages stay out of the built
site; `final` or omitted status publishes them.

## Docs and blog side by side

One project can host both: create a second site (see
[Existing Project](../creating/existing-project.md)) and give each its own `basePath` in
`codocation.yml` so they mount at different URLs, for example the docs at the root and the
blog under `/blog/`.
