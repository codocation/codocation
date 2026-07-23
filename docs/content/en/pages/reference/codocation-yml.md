---
title: codocation.yml
---

The project configuration file lives at the project root. It contains project-global settings,
the site registry, and the authoritative locale registry. Locale-owned authored and published
pages, trees, images, attachments, definitions, and sidecars live below `content/<locale>/`.

```yaml
sites:
  cdc:                                  # site id: lowercase letters, digits, dashes
    type: docs                          # docs (default) | blog
    publicUrl: https://codocation.com
    basePath: /docs                     # omit for the domain root
    homeUrl: https://codocation.com     # omit to link the title to this site's root
    externalLinksNewTab: true
    web:
      branding:
        logo: images/branding/logo.svg          # locale-owned; resolved with the page locale
        favicon: assets/media/favicon.ico        # global, non-localized browser chrome
        font: assets/fonts/Inter.woff2          # global web font file
        themeColor: "#0b6"
      customization:
        css: assets/css/site.css                # global site CSS
        js: assets/js/site.js                   # global site JavaScript
      analytics:
        enabled: true
        provider: plausible                     # plausible | ga4
        id: codocation.com
      ai:
        llmsTxt: true
        llmsFullTxt: true
        pageMarkdown: true

locales:
  en:
    title: English

publicUrl: https://codocation.com                # root fallback for sites without their own URL
pdf:
  titlePage:
    enabled: true
  font: assets/fonts/Inter.ttf                   # global PDF font
  orientation: portrait
  tableOfContents: true
  pageNumbers:
    enabled: true
    position: bottom-center

build:
  output: dist
  cleanUrls: true
```

## Notes

- **Locales**: `locales` is authoritative. When exactly one locale is configured, it is the
  default without a marker. When multiple locales are configured, exactly one must have
  `default: true`; more than one explicit default or no explicit default is rejected.
- **Several sites**: add entries under `sites:`; each site has a required tree in every
  configured locale. A site id names `<siteId>.tree.yml` and its optional sidecars.
- **Presentation metadata**: required `title` and optional `description` live at the root of
  `content/<locale>/<siteId>.tree.yml`, next to `header`, `toc`, and `footer`.
- **Global technical assets**: every configured global resource must use one of the enforced
  namespaces `assets/media/`, `assets/fonts/`, `assets/css/`, or `assets/js/`. There are no
  locale-specific favicon, font binaries, CSS, JavaScript, or analytics files in this layout.
- **Per-site web overrides**: a site entry may carry its own `web:` block; its explicit values
  override matching global fields. Web has Global and Per-site scopes, never a Locale scope.
- **Deployments**: `codocation.yml` has no deployment section. Named deployment groups live in
  project-root `deployments.yml`; tokens stay in the IDE password safe.
- **Locale sidecars**: optional files sit beside the tree:
  `content/<locale>/<siteId>.pdf.yml`, `<siteId>.seo.yml`, and `<siteId>.redirects.yml`.
  SEO resolves requested locale → default locale → built-in defaults. PDF resolves requested
  locale → default locale → `sites.<siteId>.pdf` → global `pdf` → built-in defaults. Each
  sidecar merges presence-aware leaves; a present leaf replaces the lower layer wholesale,
  with no deep or list merging. Redirects use only the requested locale and absence means an
  empty set.
- **Fallback source**: locale-aware images and attachments resolve using the page's source
  provenance. A fallback page shown in another locale uses the source locale's media, so an
  English source page cannot accidentally use a German download.

## Scope visibility and writes

- **PDF** exposes Global, Per-site, and Locale targets. Per-site is shown when the project has
  more than one site or `sites.<siteId>.pdf` already contains an explicit override. Locale is
  shown when the project has more than one locale or the requested locale's
  `<siteId>.pdf.yml` already exists. A single-site, single-locale project with neither
  override edits the global section directly, without a selector.
- **Web and Output** expose only Global and Per-site targets. Per-site is shown for a
  multi-site project or when the current site's `web` or `build` section already contains an
  explicit override. These scopes never receive a Locale target.
- **SEO and Redirects** have no scope selector: they have no global or per-site layer outside
  the selected locale sidecars. Their write target is always the current site and requested
  locale. SEO displays the requested-locale → default-locale → built-in effective value, but
  edits create or change only a sparse requested-locale override; resetting a leaf deletes only
  that override and restores fallback.
- **SEO `metaTags`** is one list-valued leaf. Lists never merge by element, so the first edit
  to an inherited row materializes the complete effective list in the requested-locale sidecar
  before applying the row change. Resetting `metaTags` removes that whole requested leaf.
- **Redirects** read and write only the requested-locale file. Resetting redirects deletes that
  file or leaves the requested redirect set empty.
- **Locale PDF writes** target the requested locale's PDF sidecar. For a non-default requested
  locale they never write the default-locale sidecar; the default locale's own sidecar is its
  legitimate target. Reset on any layer deletes only that layer's override, and switching
  scope never moves configuration between layers.

## deployments.yml

Named deployment groups are stored separately from content and presentation configuration:

```yaml
deployments:
  production:
    name: Production documentation
    sites:
      - cdc
    provider: cloudflare-pages
    accountId: <account id>
    project: codocation
  github-mirror:
    name: GitHub mirror
    sites:
      - cdc
    provider: github-pages
    repository: company/documentation
    branch: gh-pages
    path: docs/
```

Each group has a stable mapping key, a visible `name`, and at least one site ID. Cloudflare Pages
groups identify an account and project. GitHub Pages groups identify a repository and branch, with
an optional publish path. A site may belong to more than one group. Two groups cannot target the
same normalized destination, and their selected sites must not collide in the generated snapshot.
