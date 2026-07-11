---
title: codocation.yml
---

The project configuration file, at the project root. The "Site & Export" tool window and
`Settings → Tools → Codocation` edit it; hand edits are equally fine. Only `sites` and
`build` are required.

```yaml
sites:
  cdc:                        # site id: lowercase letters, digits, dashes
    title: Codocation         # required: the site name
    type: docs                # docs (default) | blog
    description: One line about the site.
    publicUrl: https://codocation.com
    basePath: null            # mount path under the domain, e.g. "blog" for /blog/
    homeUrl: null             # explicit header "home" link target; default = site root
    externalLinksNewTab: true # external links open in a new tab
    web:
      branding:
        logo: images/logo.svg           # header logo, copied into site assets
        logoAlt: Codocation             # its alt text
        favicon: images/favicon.png     # browser-tab icon
        font: assets/fonts/Inter.woff2  # a font FILE under content/assets/fonts/, not a family name
        themeColor: "#0b6"              # theme-color meta tag
      customization:
        css: assets/site.css            # linked into every page, a file under content/assets/
        js: assets/site.js              # linked into every page, a file under content/assets/
      analytics:
        enabled: true
        provider: plausible             # plausible | ga4
        id: codocation.com              # domain (plausible) or measurement id (ga4)
      ai:
        llmsTxt: true                   # emit llms.txt
        llmsFullTxt: true               # emit llms-full.txt
        pageMarkdown: true              # emit a .md mirror of every page

build:
  output: dist                # build output directory
  cleanUrls: true             # /page/ instead of /page.html

pdf:
  titlePage:
    enabled: true
    logo: images/logo.svg     # optional cover logo
    title: null               # cover title; default = site title
    subtitle: null
  font: assets/fonts/Inter.ttf # cover/body font FILE (ttf or otf), not a family name
  orientation: portrait       # portrait | landscape
  tableOfContents: true
  pageNumbers:
    enabled: true
    format: "{page} of {total}"
    position: bottom-center   # bottom-left | bottom-center | bottom-right

deploy:
  default: cloudflare         # cloudflare | gh-pages
  cloudflare:
    accountId: <32 hex characters>   # not a secret; the token lives in the IDE password safe
    project: codocation
    branch: main
  ghPages:
    repo: null                # owner/name; autodetected from the git remote when null
    branch: gh-pages
```

## Notes

- **Several sites**: add more entries under `sites:`; each needs its own id and, when
  served from one domain, its own `basePath`.
- **Per-site overrides**: a site entry may carry its own `pdf:`, `build:`, and `web:` blocks
  that override the global ones for that site.
- **`deploy:`** appears after the first successful deploy setup; you rarely write it by
  hand. Secrets never live here: tokens are stored in the IDE's password safe.
- **SEO and redirects are not in `codocation.yml`**: each site's meta tags, `robots.txt`, and
  sitemap toggle live in its own `content/<site id>.seo.yml`, and its URL redirects in
  `content/<site id>.redirects.yml`. Both are individual per site, edited from the "SEO" and
  "Redirects" tabs of the "Site & Export" tool window. See [SEO and Crawlers](../site/seo.md).
