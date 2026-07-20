---
title: SEO and Crawlers
---

The build emits what search engines and language models expect from a static site, and page
frontmatter gives per-page control where it matters.

## Per page

- `seo-description` sets the page's meta description. A page without one emits no
  `<meta name="description">` at all; the site's `description` is not a fallback here (it
  feeds the site footer and the `llms.txt` summary instead).
- `noindex: true` emits `<meta name="robots" content="noindex, nofollow">` on that page only.
- `canonical` supplies a canonical URL when the same content exists elsewhere and the other
  copy should win.

```yaml
---
title: Installation
seo-description: Install the Codocation plugin into IntelliJ IDEA.
---
```

## Per locale and site

The "SEO" tab writes sparse overrides to the requested locale's
`content/<locale>/<siteId>.seo.yml`. Its effective leaves resolve through a fixed chain:

`requested locale → default locale → built-in defaults`

A missing leaf inherits lower layers; a present leaf replaces that leaf wholesale. There is no
deep or list merging. Editing or resetting SEO changes only the requested-locale override, so
editing a German site never writes to an English fallback source. The first edit to inherited
`metaTags` materializes the complete effective list because lists are one leaf and never merge
by element.

The sidecar can switch a site out of search engines (a staging deployment or internal manual),
add `name` or `property` plus `content` meta-tag pairs emitted into every page's `<head>`,
enable `robots.txt`, and enable or disable `sitemap.xml`. When enabled, `robots.txt` allows
all crawlers and includes a `Sitemap:` line pointing at the sitemap.

## Emitted for crawlers and language models

- `sitemap.xml` contains every published page with its absolute URL, built from the site's
  `publicUrl`.
- `robots.txt` is emitted when enabled.
- `llms.txt` is a curated index following the [llms.txt proposal](https://llmstxt.org): the
  site title, one-line summary, and a link list per navigation section with each page's title,
  URL, and `seo-description` when one exists.
- `llms-full.txt` contains every page as a heading, its URL, and its full body, one after
  another.
- A Markdown mirror can be emitted for every published page at the same URL with `.md`
  appended (`/getting-started/` becomes `/getting-started.md`; the home page becomes
  `/index.md`).

The AI outputs are controlled by `web.ai.llmsTxt`, `web.ai.llmsFullTxt`, and
`web.ai.pageMarkdown` in `codocation.yml`, globally or per site. Unpublished and `noindex`
pages stay out of all three AI/Markdown outputs, as do paginated duplicates. All three are on
by default. Set `publicUrl` so `sitemap.xml`, `llms.txt`, and `llms-full.txt` carry absolute
URLs; without it their entries are root-relative paths.

## Redirects

The "Redirects" tab writes only the requested locale's
`content/<locale>/<siteId>.redirects.yml`, with `from`/`to` pairs and an optional description.
Redirects never fall back or merge across locales; absence means an empty redirect set.
Each rule is emitted to a Cloudflare Pages `_redirects` file at the build output root as one
line in the `<from> <to> 301` format Cloudflare Pages reads.
