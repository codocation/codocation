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

A build **replaces** the output of the sites it builds: it clears `<output>/<site id>` and writes
the site fresh, so a page you renamed stops serving its old URL and a language you removed stops
being published. Other sites in the same output directory are untouched, and so is anything you keep
beside them. The clear happens only after the site assembles successfully, so a build that fails on
a broken page leaves the previous output in place rather than deleting it.

## What ends up in the output

- One HTML file per published page, styled by `codocation.css`.
- `search-index.json` and `search-runtime.js`: the built-in client-side search. It works on
  the published site with no external service.
- `sitemap.xml` and `llms.txt` for crawlers and language models.
- Referenced locale images and attachments, plus global technical assets, copied into matching
  output namespaces: `content/<locale>/images/*` to `images/*`,
  `content/<locale>/attachments/*` to `attachments/*`, and `assets/media/*`,
  `assets/fonts/*`, `assets/css/*`, and `assets/js/*` to their corresponding output paths.
  Locale images and attachments follow the page's source provenance, so a page resolved from the
  default language uses that language's media.

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

Build consumes the requested site's declared locale membership. The CLI builds every configured
locale. In the IDE, Free builds and validates only each site's effective default locale, while Pro
processes every declared membership. Manage Locales remains available in Free for renaming, changing
the default, and removing existing locales; adding locale memberships through the UI requires Pro.

## When a build refuses to run

Two guards keep a broken site from being published:

- The site must have a home page: either a page marked as home in the navigation tree, or an
  `index.md` at the top level. Without one, the site root would be an empty URL, and the
  build stops with an error that names the missing piece.
- Validation findings with the "error" severity (broken page references in the navigation
  tree, for example) also stop the build. Fix them in the "Problems" tool window, or adjust
  severities in `error-registry.yml` if a rule should not block you.

Pair trees and sidecars are required only for declared site memberships. An undeclared pair is an
ERROR and ignored, as is a content directory whose locale is absent from the root catalog; an
inactive catalog entry is also invalid under the strict catalog union.

## Export as ZIP

`Tools → Codocation → Export ZIP` runs the same build and packages the output into a single
`.zip` archive: convenient for handing the site to a hosting pipeline that expects an upload.
