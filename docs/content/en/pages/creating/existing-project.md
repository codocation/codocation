---
title: Existing Project
---

Use this path when the documentation should live next to your code, in the repository you
already have open.

## Create from the tool window

1. Open the "Codocation" tool window.
2. Click "Create Documentation".
3. Fill in the documentation title, the site id, the type ("Docs" or "Blog"), and optionally
   a description, then confirm.

The files are created in your project root, and the tool window switches to the new
navigation tree. Nothing else in your project is touched: Codocation adds `codocation.yml`,
the locale-owned `content/<locale>/` directory, root `definitions/`, and the global
`assets/media/`, `assets/fonts/`, `assets/css/`, and `assets/js/` namespaces as needed. Root
`locales` is the canonical catalog of codes, order, and Titles; `sites.<siteId>.locales` records
which variants this site publishes, and `sites.<siteId>.defaultLocale` owns its unprefixed routes
and content fallback. Pages, images, attachments, site trees, sidecars, and translated definition
payloads belong to the declared locale/site pairs.

## More than one site

A project can host several documentations: for example, a docs site and a blog side by side,
or separate manuals for separate products. Run "Create Documentation" again to add another
site; each gets its own site id, locale trees and site files, and entry in `codocation.yml`. The
"Codocation" tool window switches between sites, and build, export, and deploy act on the
selected one.

## Existing locales and Free/Pro installation

Every Create Documentation path, including an additional site, shows locale controls. Free can
select exactly one existing catalog locale for a new site's membership and can build, validate, and
preview all valid locales. Pro owns structured catalog and membership management, site-default and
definitions-fallback changes, and mutation fixes. A pair tree or sidecar is valid only when the
locale is declared for that site; unknown locale directories and undeclared pairs are reported as
ERROR and ignored.

A pre-existing multi-locale project remains readable, buildable, validatable, and previewable
without Pro. Free does not rewrite or remove its locale data. A repository installation of Pro
resolves its mandatory Free dependency. Installing a local Pro ZIP requires Free to be installed
already or discoverable from a configured repository.
