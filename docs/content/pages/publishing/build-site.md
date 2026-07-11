---
title: Build the Site
---

Building assembles your Markdown, navigation, and theme into a static site: plain HTML, CSS,
and a few site files, with no server-side code. The result can be hosted anywhere static
files are served.

## Run a build

Choose `Tools → Codocation → Build HTML Site` (the same actions are available on the main
toolbar). The build runs in the background and reports the number of pages written and the
output directory when it finishes.

## What ends up in the output

- One HTML file per published page, styled by `codocation.css`.
- `search-index.json` and `search-runtime.js`: the built-in client-side search. It works on
  the published site with no external service.
- `sitemap.xml` and `llms.txt` for crawlers and language models.
- Referenced images, attachments, and assets, copied into the matching output directory:
  `content/images/*` to `images/*`, `content/attachments/*` to `attachments/*`, and
  `content/assets/*` (including `content/assets/fonts/*`) to `assets/*`.

## Output settings

The "Output" tab of the "Site & Export" tool window controls the build:

- "Output directory": where the site is written. The default is `dist`.
- "Clean URLs (drop .html)": publish pages as `/getting-started/` instead of
  `/getting-started.html`.

The same values live under `build:` in `codocation.yml`:

```yaml
build:
  output: dist
  cleanUrls: true
```

## When a build refuses to run

Two guards keep a broken site from being published:

- The site must have a home page: either a page marked as home in the navigation tree, or an
  `index.md` at the top level. Without one, the site root would be an empty URL, and the
  build stops with an error that names the missing piece.
- Validation findings with the "error" severity (broken page references in the navigation
  tree, for example) also stop the build. Fix them in the "Problems" tool window, or adjust
  severities in `error-registry.yml` if a rule should not block you.

## Export as ZIP

`Tools → Codocation → Export ZIP` runs the same build and packages the output into a single
`.zip` archive: convenient for handing the site to a hosting pipeline that expects an upload.
