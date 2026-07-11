---
title: SEO and Crawlers
---

The build emits what search engines and language models expect from a static site, and the
frontmatter gives per-page control where it matters.

## Per page

- `seo-description` - the page's meta description. A page without one emits no
  `<meta name="description">` at all; the site's `description` from `codocation.yml` is not
  used as a fallback here (it feeds the site footer and the `llms.txt` summary instead).
- `noindex: true` - emits `<meta name="robots" content="noindex, nofollow">` on this page
  only.
- `canonical` - a canonical URL for the page, when the same content exists elsewhere and
  the other copy should win.

```yaml
---
title: Installation
seo-description: Install the Codocation plugin into IntelliJ IDEA.
---
```

## Per site

The "SEO" tab of the "Site & Export" tool window stores its settings in
`content/<site id>.seo.yml`:

- Switch a site out of search engines entirely (a staging deployment, an internal manual):
  every page then carries the robots noindex meta tag.
- Meta tags: `name` or `property` plus `content` pairs emitted into every page's `<head>` -
  site verification tags, Open Graph defaults, and the like.
- `robots.txt`: when enabled, the build emits an allow-all `robots.txt` at the site root
  with a `Sitemap:` line pointing at the sitemap.
- Sitemap: untick to skip `sitemap.xml` for this site.

## Emitted for crawlers and language models

- `sitemap.xml` - every published page with its absolute URL (built from the site's
  `publicUrl`).
- `robots.txt` - when enabled in the "SEO" tab.
- `llms.txt` - a curated index of the site for language models, at the site root next to
  `sitemap.xml`, following the [llms.txt proposal](https://llmstxt.org): the site title, a
  one-line summary, then a link list per navigation section - one entry per page with its
  title, URL, and description (the page's `seo-description`, left out when the page has
  none rather than repeating the site summary on every line).
- `llms-full.txt` - the whole site's content in one markdown file, a widely adopted
  convention (not part of the llms.txt proposal itself): every page as a heading, its URL,
  and its full body, one after another.
- A markdown mirror of every page, at the same URL with `.md` appended (`/getting-started/`
  becomes `/getting-started.md`, the home page becomes `/index.md`) - also part of the
  llms.txt proposal, for a language model that wants one page's clean source instead of the
  rendered HTML.

All three are on by default; switch any of them off in the "Web" tab, which stores
`web.ai.llmsTxt`, `web.ai.llmsFullTxt`, and `web.ai.pageMarkdown` in `codocation.yml` -
globally or per site.

All are regenerated on every build; unpublished, paginated-duplicate, and `noindex` pages
stay out of all three. Set the site's `publicUrl` so `sitemap.xml`, `llms.txt`, and
`llms-full.txt` carry absolute URLs - without it the entries are root-relative paths.

## Redirects

The "Redirects" tab of the "Site & Export" tool window stores per-site URL redirects in
`content/<site id>.redirects.yml`, each a `from`/`to` pair with an optional description. A
site with at least one rule gets a Cloudflare Pages `_redirects` file at its build output
root, one line per rule in the `<from> <to> 301` format Cloudflare Pages reads.
