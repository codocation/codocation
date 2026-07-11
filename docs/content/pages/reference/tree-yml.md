---
title: <site id>.tree.yml
---

The navigation file of a site, at `content/<site id>.tree.yml`. The "Codocation" tool
window edits it visually; the format below is what lands in the file.

```yaml
header:                        # links in the site header
  - page: pages/pricing.md
  - href: https://github.com/acme/product
    label: GitHub
    button: true               # render as a button instead of a plain link
    icon: github               # optional icon
toc:                           # the sidebar / table of contents
  - page: pages/index.md
    home: true                 # served at the site root; exactly one page may have it
  - page: pages/getting-started.md
    title: Start Here          # optional label override; default = the page's own title
  - section: Guides            # a named group, not a page
    collapsed: true            # start collapsed in the sidebar
    children:
      - page: pages/guides/install.md
      - page: pages/guides/deploy.md
  - page: pages/reference.md
    children:                  # pages can nest children directly, without a section
      - page: pages/reference/api.md
footer: []                     # links in the site footer; same item forms as header
```

## Item forms

- **Page**: `page:` with a path relative to `content/`. Optional keys: `title` (label
  override), `home` (site root marker), `hidden` (keep the entry out of the published
  navigation), `children` (nested items).
- **External link**: `href:` with `label:`; header/footer links may add `button: true`,
  `color:`, and `icon:`.
- **Section**: `section:` with a label and `children:`; sections group pages in the `toc`
  and may nest. Optional `collapsed: true`.

## Rules validation enforces

- A referenced page file must exist.
- Only one page carries `home: true`.
- `header` and `footer` are flat: no nested sections there.
- An item is one thing: `page`, `section`, or `href`, never a combination.
