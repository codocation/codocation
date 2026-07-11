---
title: Getting Started
---

The fast path from an empty IDE to a published site. Every step here has a detailed chapter;
this page only walks the straight line.

## 1. Install

Open `Settings → Plugins`, find "Codocation" in the "Marketplace" tab, install, and restart
the IDE.

## 2. Create

Go to `File → New → Project…`, pick "Codocation", choose the "Docs" type, and fill in the
project name, documentation title, and site id. The project opens with a small site already
scaffolded: a home page, a first page, and the navigation tree in the "Codocation" tool
window.

Docs should live next to your code instead? See
[creating documentation](creating/existing-project.md).

## 3. Write

Click a page in the "Codocation" tool window and type: it is plain Markdown, and the preview
shows the page as it will look on the site while you edit. Add and rearrange pages by
drag-and-drop in the tree.

The details live in the [Writing](writing/pages.md) chapter.

## 4. Publish

Choose `Tools → Codocation → Deploy Site` and follow the setup: pick "Cloudflare Pages" or
"GitHub Pages", connect the account, and deploy. The notification links to your live site.

Prefer an artifact instead of hosting? [Build the site](publishing/build-site.md) to get a
static `dist/` directory, or [export a PDF](publishing/export-pdf.md).
