---
title: New Project
---

Use the New Project wizard when the documentation is a project of its own: a standalone
docs site, a product manual, a blog.

## Create through the wizard

1. Go to `File → New → Project…` and pick "Codocation" in the generator list.
2. Choose the documentation type:
   - "Docs" - a structured documentation site with a navigation tree.
   - "Blog" - chronological posts with a listing page.
3. Fill in the project name and location.
4. Fill in the documentation title (the site name readers will see), the site id, and
   optionally a description.
5. Create the project.

## Locales in a new project

The root `locales` map is the canonical project-wide catalog: each entry supplies a locale code,
order, and display Title. A new site lists its published variants in `sites.<siteId>.locales` and
may omit `defaultLocale` when it has one member; multiple members require an explicit member
default. The bootstrap creates one catalog entry, one site membership, the effective site default,
and the locale-owned starter content together. It creates no project-global definitions fallback.

Free creates the initial one-locale site and ordinarily validates, previews, and builds each site's
effective default. "Manage Locales" remains available for existing locales: Free can rename Title
or Code, change a site default, and remove memberships or locales. Without Pro, "Predefined
Locales" and "Add Custom Locale" remain visible, collapsed, disabled, and marked with the `Pro`
badge; adding a locale through the IDE requires Pro. The CLI continues to process all configured
variants.

## Choosing a site id

The site id is a short technical identifier: lowercase letters, digits, and dashes. It names
the navigation file (`content/<locale>/<site id>.tree.yml`) and distinguishes sites when a project
hosts more than one. Pick something short and stable; the human-facing name is the title,
which you can change at any time. The site id itself can also be changed later, from "Edit
Documentation..." in the "Codocation" tool window; see [Navigation](../writing/navigation.md).

## Description

The description is optional and can be filled in now or later; it is a one-line summary
shown in the site footer. See [Titles, URLs, and Branding](../site/branding.md).

## What you get

The wizard scaffolds a working site and opens it: a home page, a starter page, the navigation
tree, and `codocation.yml` with sensible defaults. See
[project structure](project-structure.md) for the full inventory.
