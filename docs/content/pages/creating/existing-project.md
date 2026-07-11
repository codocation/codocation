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
the `content/` directory, and `commons/` for shared definitions, and works only inside them.

## More than one site

A project can host several documentations: for example, a docs site and a blog side by side,
or separate manuals for separate products. Run "Create Documentation" again to add another
site; each gets its own site id, navigation tree, and entry in `codocation.yml`. The
"Codocation" tool window switches between sites, and build, export, and deploy act on the
selected one.
